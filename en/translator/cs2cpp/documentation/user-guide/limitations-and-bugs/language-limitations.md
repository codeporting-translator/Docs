# Language Limitations #

This page lists language issues that arise when translating from C# to C++. These issues are either fundamentally unsolvable due to the different approaches used in the languages, or require excessively heavyweight support from the translator or system library, and are therefore unlikely to be addressed in the foreseeable future.

## Virtual and interface generic methods ##

Virtual methods (including interface methods that are translated into virtual ones) cannot be templated in C++. This is a fundamental property of the language, and there is no universally applicable workaround for it that could be applied in automatic translation. Therefore, when encountering this error, it is recommended to either refactor the C# source code or use attributes where available.

```cs
interface IInterface
{
    void Set<T>(T t);
}
```

Code above may be converted to

```cpp
class IInterface
{
    template<typename T>
    virtual void Set(T t) = 0;
};
```

which should never be compiled.

## Nested types inheriting from surrounding types ##

The code below is correct in C#:

```cs
public class CommonNode
{
    private class DerivedNode : CommonNode
    {
    }
}
```

But when translated to C++:

```cpp
class CommonNode
{
private:
    class DerivedNode : public CommonNode
    {
    };
};
```

we get a compilation error because `CommonNode` is an incomplete type during `DerivedNode` compilation.

## Circular type dependencies and other declaration order issues ##

In C#, the order in which symbols are defined is generally unimportant, as long as the symbol is defined at all. In C++, references to undefined or undeclared symbols are not allowed. The compiler attempts to resolve such situations automatically, you can help him using various attributes and configuration options, but this problem is not 100% solved in general and may require source code refactoring, including manually separating symbols into different files.

Here, for example, is a completely correct (albeit synthetic) code for C#.

```cs
public class A
{
    public B.Nested b;

    public struct Nested
    {
        public int x;
    }
}

public class B
{
    public A.Nested a;

    public struct Nested
    {
        public int x;
    }
}
```

However, for C++ this is an unsolvable puzzle: to define class A you need to know the definition of class B and vice versa. In reality, of course, there are many more such problems, and they are not as fundamental as shown above, but they can still create problems when translating code and may require manual intervention in the process.

## Memory management issues ##

Because C# has a garbage collector and C++ has reference counting, we have problems with circular references.

```cs
public class A
{
    public B BRef;
}

public class B
{
    public A ARef;
}
```

In C++ translation, those references are often represented via `System::SharedPtr`, which can create a reference cycle and prevent objects from being released:

```cpp
class A : public System::Object
{
public:
    System::SharedPtr<B> BRef;
};

class B : public System::Object
{
public:
    System::SharedPtr<A> ARef;
};
```

It is necessary to use the special attribute [CppWeakPtr](../cpp-attributes/reference.md#cppweakptr) to indicate which object actually owns another object and which one only references it.

Also, some C# APIs related to memory management and garbage collection are meaningless in C++. Most of them are not implemented, and those that are implemented are stubs that merely simulate their presence and do nothing.

## Reflection issues ##

Although a small portion of reflection is implemented in the compiler's system library, some elements of reflection are fundamentally unimplementable in C++. For example, in C#, you can dynamically parameterize a generic type and then instantiate it. This is obviously impossible in C++, as it would require compiling the template source code at runtime.

```cs
Type genericList = typeof(System.Collections.Generic.List<>);
Type listOfInt = genericList.MakeGenericType(typeof(int));
object instance = Activator.CreateInstance(listOfInt);
```

## 'new' modifier in method declaration is not supported ##

Translator does not support the **new** modifier in method declarations because there is no available equivalent in C++. 

```cs
public class Base
{
    public virtual void F()
    {
    }
}

public class Derived : Base
{
    public virtual new void F()
    {
    }
}
```

Translator ignores the **new** modifier in method declarations and translates the method declaration as if it had no **new** modifier. Therefore, in the fragment above, translated to C++, the function `F` of class `Derived` will override the function `F` of class `Base` and will be called in the code below (which will not happen in the original C# code).

```cpp
System::SharedPtr<Base> base = System::MakeObject<Derived>();
base->F();
```

It is recommended to manually rename `new` methods or use appropriate attributes.

## Variant and covariant type parameters are translated as invariant type parameters ##

Variance annotations — keywords **in** and **out** in variant type parameter lists — are ignored by the translator. All covariant and contravariant type parameters of an interface or a delegate encountered in C# code by the translator are interpreted and translated as invariant type parameters.

```cs
public interface IReadOnlyList<out T>
{
    T Get(int index);
}

public interface IComparer<in T>
{
    int Compare(T x, T y);
}
```

## Overloaded generic types ##

In C#, a generic type can have a variable number of parameters, including none at all.

```cs
public class Foo
{
}

public class Foo<T>
{
}

public class Foo<T1, T2>
{
}
```

 In C++, this is not allowed, leading to unpleasant collisions and generally not having a good solution. The translator can attempt to emulate overloading via variadic template specialization or rename the delegate by appending the number of arguments to the class name. However, manual renaming is generally recommended.

## Translation of invocation of virtual method in a class constructor may not preserve semantics ##

Translator translates invocation of a virtual method in a C# class constructor into invocation of a virtual method in a C++ class constructor without emulating rules applied in C# to resolve which virtual method version will be invoked. This may result in a runtime difference between behavior of a C# class hierarchy and the corresponding C++ class hierarchy generated by the translator. In the following example, at runtime during instantiation of class `B` in method `Main()`, `B.f()` will be invoked in the constructor of class `A`:

```cs
namespace ns
{
    class A
    {
        public A()
        {
            f();
        }

        public virtual void f()
        {}
    }

    class B : A
    {
        public override void f()
        {}
    }

    class App
    {
        public static int Main(string[] args)
        {
            B b = new en.B();
            return 0;
        }
    }
}
```

but in the corresponding C++ code generated by the translator, method `A::f()` will be invoked in the constructor of class `A`:

```cpp
namespace ns {

class A : public System::Object
{
public:
    A()
    {
        f();
    };

    virtual void f() {};
};

class B : public A
{
public:
    virtual void f() {};
};

class App : public System::Object
{
public:
    static int32_t Main(System::ArrayPtr<System::String> args)
    {
        System::SharedPtr<B> b = System::MakeObject<en::B>();
        return 0;
    };
};

}
```

## Argument evaluation order issues ##

In C#, the order of argument evaluation is strictly defined, but in C++ it is not. In some cases, this can lead to different runtime behavior between the original and translated code.

```cs
int i = 0;
void M(int x, int y) { }

M(i++, i++);
```

In C#, arguments are evaluated left to right, so the first call uses `0` and increments `i` to `1` before the second call. In C++, the order of evaluation is unspecified and the runtime values may differ.

## 'internal' access modifier has no analog in C++ ##

Because in C++ there is no analog of C#'s **internal** access modifier, internal members of C# classes are translated as protected or public (when the related [config option](../configuration-file/options.md#internal_as_public) is turned on). Thus the following C# class

```cs
class A
{
    internal int a = 1;
    internal void F() { }
}
```

will be translated into the following C++ class (when `internal_as_public` is on)

```cpp
class A : public System::Object
{
public:
    int32_t a;
    void F();
    A();
};
```

or to the following otherwise:

```cpp
class A : public System::Object
{
    friend class ClassWhichUsesA;

public:
    A();

protected:
    int32_t a;
    void F();
};
```

For **internal** classes declared in namespaces, this modifier is ignored entirely.

## Expression trees ##

Proper processing of expression trees requires significant framework support, including robust reflection and just-in-time compilation.

```cs
Expression<Func<string, int>> expr = s => s.Length;
var member = (MemberExpression)expr.Body;
Console.WriteLine(member.Member.Name);
```

C++ doesn't provide these capabilities and won't in the foreseeable future.
