# Translator Limitations and Bugs #

This Page Contains Translator limitations and bugs.

## Type parameter constraints in delegate declaration are not supported ##

Translator ignores type parameter constraints in delegate declaration.

So the following C# code that declares two generic delegates one of which contains specification of type constraints

```cs
namespace ns
{
    public delegate TOut D1<TIn, TOut>(TIn value);
    public delegate TOut D2<TIn, TOut>(TIn value) where TIn : ICloneable where TOut : IConvertible;
}
```

will be translated into two basically identical C++ declarations

```cpp
namespace ns
{
    template <typename TIn, typename TOut> using D1 = System::MulticastDelegate<TOut(TIn)>;
    template <typename TIn, typename TOut> using D2 = System::MulticastDelegate<TOut(TIn)>;
}
```

This doesn't affect the functionality of the translated code, but if the user writes their own C++ code, it will allow them to use a generic delegate with inappropriate types. This problem can be solved within the C++ version used in the library using SFINAE, but this will make the delegate declaration code cumbersome and difficult to read. Perhaps when we switch to C++20 with its contracts, this can be done more elegantly.

## There is a limitation imposed on switch statement with string as governing type ##

If **string** is the governing type of **switch expression** in C# **switch statement**, such **switch statement** is translated into equivallent system of nested **if-else statements** in C++, if default translator configuration is applied. The number of levels in resulting system of nested **if-else statements** is equal to the number of **switch sections** in a **switch statement** in C# code. Thus the following dummy C# **switch statement**

```cs
string s = "two";
switch (s)
{
    case "one": break;
    case "two": break;
    case "three": break;
    default: break;
}
```

will be translated into following C++ code

```cpp
System::String s = u"two";
const System::String switch_value_0 = s;

if (switch_value_0 == u"one")
{
}
else if (switch_value_0 == u"two")
{
}
else if (switch_value_0 == u"three")
{
}
else if (true)
{
}
```

The problem arises when the number of **switch sections** in a C# **switch statement** is greater than 127. Because some C++ compilers support no more than 127 levels of nesting in a system of **if-else statements**, the generated C++ code may not compile. To overcome this limitation one may set translator's option **alternative_string_switch**. This will force translator to translate C# **switch statement** into a set of non-nested simple C++ **if statements** all wrappend in a **do-while loop**. Thus with option **alternative_string_switch** set translator will translate the sample C# **switch statement** into the following C++ code

```cpp
System::String s = u"two";
{
    const System::String switch_value_0 = s;
    do {
        if (switch_value_0 == u"one")
        {
            break;
        }
        if (switch_value_0 == u"two")
        {
            break;
        }
        if (switch_value_0 == u"three")
        {
            break;
        }
        if (true)
        {
            break;
        }
    } while (false);
}
```

The second approach is free from the limitations of the first, but introduces its own. For examlpe if in input C# code **switch statement** is located in the body of loop statement and one or more **switch sections** have **continue** statement in their bodies, translator will fail to translate this code into semantically equivalent C++ code.

## Class' method named Type() in C# code may lead to name collisions in generated C++ code ##

Translator includes following static method in every generated C++ class

```cpp
static const System::TypeInfo& Type();
```

This may lead to name collisions when translating C# classes that have method named Type.

## C# finally blocks containing code that may throw exceptions are translated in unsafe C++ code that may throw exceptions from destructor ##

Translator translates C# code in **finally** block into equivalent C++ code, which is put into a destructor of a special service object. This means that in code in C# **finally** block may throws an execpion, this same excetpion may potentially be thrown in destructor of a service object in generated C++ code, which can be disasterous for the C++ application. Thus the following C# code will be translated into unsafe C++ code and should be rewritten so that its **finally** block does not throw before passing the code to translator for translation

```cs
// NOTE: following C# code is an example of code that should NOT be passed to translator.
try
{
    A a = new A();
}
finally
{
    // Following line is bad because it will be translated into C++ code
    // that throws from destructor.
    throw new NullReferenceException();
}
```

## On C++ side System.String is value type ##

Unlike C#, in C++, System::String is a value type, albeit one with a null state. This leads to numerous minor inconsistencies, redundant copying, and performance differences between the original and translated code. For example, when calling string methods, the null state is not always checked (apparently for performance reasons). Thus the following sample C# method when executed will trigger a `NullReferenceException` exception

```cs
void Foo()
{
    string s = null;
    s.IndexOf('a');
}
```

when translated it will turn into following C++ method

```cpp
void Bar::Foo()
{
    System::String s;
    s.IndexOf('s');
}
```

which does not trigger any exceptions.

For the same reason translator does not support **lock** statement that accepts a string object. Translator will traslate such statement into syntactically incorrect C++ code.

## Translator translates embedded resources only from one project of a C# solution being translated ##

If a C# solution consists of a single project that has embedded resources, the resources will be translated and embedded in generated C++ project correctly. However, if a C# solution consists of multiple projects, two or more of which have embedded resources, Translator will translate resources from only one of them. Resources from the rest of the projects will be ignored.

## Method Object.MemberwiseClone() may behave differently in C# code and generated C++ code ##

In C# method `MemberwiseClone()` performs shallow copy of current object regardless of the class in the context of which the method is invoked. Thus in the following example method `MemberwiseClone()` called in method `A.MA1()` will return a reference to an instance of class B, despite the fact it is invoked in a method of class A

```cs
namespace ns
{
    abstract class A
    {
        public void MA1()
        {
            Object o = MemberwiseClone();
        }

        abstract public void MA2();

        public int m_a = 0;
    }

    class B : A
    {
        // Dummy implementation of abstract method inherited from A.
        public override void MA2() { }

        public int m_b = 1;
    }

    class App
    {
        public static int Main(string[] args)
        {
            A b = new B();
            b.MA1();

            return 0;
        }
    }
}
```

If this code is passed to translator, it will generate C++ code that behaves differently

```cpp
namespace ns {

class A : public System::Object
{
public:
    int32_t m_a;

    void MA1()
    {
        System::SharedPtr<System::Object> o = System::MemberwiseClone(this);
    }

    virtual void MA2() = 0;
    A() {};
};

class B : public A
{
public:
    int32_t m_b;
    virtual void MA2(){};
    B() : m_b(1) {};
};

class App : public System::Object
{
public:

    static int32_t Main(System::ArrayPtr<System::String> args)
    {
        System::SharedPtr<A> b = System::MakeObject<B>();
        b->MA1();
    }};
}
```

This C++ code is syntactically incorrect, because here method `MemberwiseClone()` invoked in method `A::MA1()` will try to return an instantiate and return an instance of class A (not B as it is in C#), but will fail because class A is abstract.

Translator option `polymorphic_memberwiseclone` will enforce C#-like behavior of method `MemberwiseClone()`.

## Compilers may generate warnings when processing translated C++ code ##

C++ code generated by Translator may not meet all the strictest requirements imposed by supported compilers, which may lead to warnings generated by compilers when processing the code.

## Wrong instances of const methods can be called ##

If library declares some virtual method with 'const' qualifier and C# code doesn't have [CppConstMethod](../cpp-attributes/reference.md#cppconstmethod) attribute applied to implementation of this method, incorrect version of this method will be called. If the method is declared as abstract, the class becomes abstract, too.

## Unions are not supported ##

Unions declared using `StructLayout(LayoutKind.Explicit)` attribute are not supported.

## NUnit's Assert.Catch() method support is limited ##

Translator provides limited support of Assert.Catch() method. Only two following overloads of Assert.Catch() method are partially supported and are translated in syntactically correct (but not semantically equivallent) C++ code:

* Exception Catch(TestDelegate code);
* T Catch\<T\>(TestDelegate code) where T : Exception;

All other overloads are not supported and are translated into syntactically incorrect C++ code.

Invocation of either of two supported overloads in C# code is translated into invocation of the same C++ static method bool AssertThrows(std::function<void()> func). So the following C# code

```cs
Assert.Catch(SomeClass.SomeMethod);
Assert.Catch<NullReferenceException>(SomeClass.SomeMethod);

```

will be translated into the following two identical lines of C++ code

```cpp
System::TestTools::AssertThrows(SomeClass::SomeMethod);
System::TestTools::AssertThrows(SomeClass::SomeMethod);

```

Note that C++ method `System::TestTools::AssertThrows()` is semantically different from `Assert.Catch()` in that it catches *all* exceptions and it returns a **bool** value indicating if *any* exeception was caught, while NUnit's `Assert.Catch()` returns a caught Exception object.

## NUnit's Assert.Greater() method support is limited ##

Following Assert.Greater() method's overloads that accept reference to IComparable interface are *not* supported by Translator

* void Greater(IComparable arg1, IComparable arg2);
* void Greater(IComparable arg1, IComparable arg2, string message);
* void Greater(IComparable arg1, IComparable arg2, string message, params object[] args);

Invocation of these overloads are translated into syntactically incorrect C++ code.

## NUnit's Assert.GreaterOrEqual() method support is limited ##

Following Assert.GreaterOrEqual() method's overloads that accept reference to IComparable interface are *not* supported by Translator

* void GreaterOrEqual(IComparable arg1, IComparable arg2);
* void GreaterOrEqual(IComparable arg1, IComparable arg2, string message);
* void GreaterOrEqual(IComparable arg1, IComparable arg2, string message, params object[] args);

Invocation of these overloads are translated into syntactically incorrect C++ code.

## NUnit's Assert.Ignore() method is not supported ##

Method Assert.Ignore() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.Ignore() method in C# code.

## NUnit's Assert.Inconclusive() method is not supported ##

Method Assert.Inconclusive() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.Inconclusive() method in C# code.

## NUnit's Assert.IsAssignableFrom() method is not supported ##

Method Assert.IsAssignableFrom() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.IsAssignableFrom() method in C# code.

## NUnit's Assert.IsInstanceOf() method is not supported ##

Method Assert.IsInstanceOf() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.IsInstanceOf() method in C# code.

## NUnit's Assert.IsInstanceOfType() method is not supported ##

Method Assert.IsInstanceOfType() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.IsInstanceOfType() method in C# code.

## NUnit's Assert.IsNaN() method is not supported ##

Method Assert.IsNaN() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.IsNaN() method in C# code.

## NUnit's Assert.IsNotAssignableFrom() method is not supported ##

Method Assert.IsNotAssignableFrom() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.IsNotAssignableFrom() method in C# code.

## NUnit's Assert.IsNotInstanceOf() method is not supported ##

Method Assert.IsNotInstanceOf() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.IsNotInstanceOf() method in C# code.

## NUnit's Assert.IsNotInstanceOfType() method is not supported ##

Method Assert.IsNotInstanceOfType() is not supported by Translator. Translator will generate an error message when encounters invocation of Assert.IsNotInstanceOfType() method in C# code.

## NUnit's Assert.Less() method support is limited ##

Following Assert.Less() method's overloads that accept reference to IComparable interface are *not* supported by Translator

* void Less(IComparable arg1, IComparable arg2);
* void Less(IComparable arg1, IComparable arg2, string message);
* void Less(IComparable arg1, IComparable arg2, string message, params object[] args);

Invocation of these overloads are translated into syntactically incorrect C++ code.

## NUnit's Assert.LessOrEqual() method support is limited ##

Following Assert.LessOrEqual() method's overloads that accept reference to IComparable interface are not supported by Translator

* void LessOrEqual(IComparable arg1, IComparable arg2);
* void LessOrEqual(IComparable arg1, IComparable arg2, string message);
* void LessOrEqual(IComparable arg1, IComparable arg2, string message, params object[] args);

Invocation of these overloads are translated into syntactically incorrect C++ code.

## NUnit's Assert.That() method support is limited ##

Translator does not support following four overloads of method Assert.That()

* void That(TestDelegate code, IResolveConstraint constraint);
* void That\<T>(ActualValueDelegate\<T> del, IResolveConstraint expr);
* void That\<T>(ActualValueDelegate\<T> del, IResolveConstraint expr, string message);
* void That\<T>(ActualValueDelegate\<T> del, IResolveConstraint expr, string message, params object[] args);

All four are translated to syntactically incorrect C++ code.

## Support of NUnit's constraint-based assertion model is limited ##

Translator supports only four types of constraints used in NUnit's Constraint-based assertion model. Following three of them are completely supported:

* TrueConstraint
* FalseConstraint
* NullConstraint

The fourth constraint type - EqualsConstraint - has quite limited support. It is only translated when invocation expression Is.Equals() is used as the second argument of Assert.That() method.

So the following C# code will be correctly translated

```cs
Assert.That(5, Is.EqualTo(5));
```

while translation of this C# code will fail

```cs
NUnit.Framework.Constraints.EqualConstraint ec = Is.EqualTo(5);
Assert.That(5, ec);
```

as well as translation of this code

```cs
Assert.That(5, new NUnit.Framework.Constraints.EqualConstraint(5));
```

Also, no modifiers for EqualsConstraint are supproted.

## NUnit's Assert.Throws() method support is limited ##

Out of nine overloads of Assert.Throws() method (including three generic ones) defined by NUnit framework Translator supports following six

* Exception Throws(Type expectedExceptionType, TestDelegate code);
* Exception Throws(Type expectedExceptionType, TestDelegate code, string message);
* Exception Throws(Type expectedExceptionType, TestDelegate code, string message, params object[] args);
* T Throws\<T>(TestDelegate code) where T : Exception;
* T Throws\<T>(TestDelegate code, string message) where T : Exception;
* T Throws\<T>(TestDelegate code, string message, params object[] args) where T : Exception;

Other three overloads that take reference to IResolveConstraint interface as their first argument are not supported.

Also Translator does not support TestDelegate delegate type declared by NUnit framework. C# code that explicitly mentions this type by name will be translated into syntactically incorrect C++ code. Thus, following C# code is *not* supported by Translator due to explicit mentioning of TestDelegate type

```cs
using System;
using NUnit.Framework;

namespace ns
{
    class Tested
    {
        public static void M()
        {
            throw new NullReferenceException();
        }
    }

    [TestFixture]
    class Tests
    {
        [Test]
        public void Test()
        {
            // Following line is not supported by Translator and will be translated into
            // incorrect C++ code.
            Assert.Throws<NullReferenceException>(new TestDelegate(Tested.M));
        }
    }
}
```

To overcome this limitation one can use anonymous delegates

```cs
[Test]
public void Test()
{
    // Following line will be translated correctly.
    Assert.Throws<NullReferenceException>(delegate {Tested.M();});
}
```

or rely on implicit casting

```cs
[Test]
public void Test()
{
    // Following line will be translated correctly.
    Assert.Throws<NullReferenceException>(Tested.M);
}
```

## NUnit's ExpectedException attribute support is limited ##

There are some limitations with regard to support of ExpectedException method attribute.

Out of six parameters of ExpectedException attribute, only following three are supported:

* ExpectedException
* ExpectedExceptionName
* ExpectedMessage

However, only one of the three supported parameters can be specified for each method marked with ExpectedException attribute. Either named or unnamed, only the first parameter from the list of parameters passed to ExpectedException attribute constructor will be processed by Translator. The rest of the parameters if present will be ignored.

Thus each of the following definitions of method M() in C#

```cs
[ExpectedException(ExpectedException # typeof(NullReferenceException), ExpectedMessage # "Message")]
public void M()
{
}
```

```cs
[ExpectedException("System.NullReferenceException"))]
public void M()
{
}
```

will be translated into identical C++ code

```cpp
TEST_F(Tests, M)
{
    ASSERT_THROW({
        M();
    }, System::NullReferenceException);
}
```

ExpectedMessage parameter is ignored by Translator because it goes second in the list of the attribute parameters.

The following definition in C#

```cs
[ExpectedException(ExpectedMessage = "Message", ExpectedException = typeof(NullReferenceException))]
public void M()
{
}
```

will be translated into following C++ code

```cpp
TEST_F(Tests, M)
{
    try
    {
        M();
    }
    catch (System::Exception& ex)
    {
        ASSERT_EQ(u"Message", ex.get_Message());
    }
}
```

In this example time ExpectedException parameter is ignored by Translator because it goes second in the list of attribute's parameters.

## NUnit's Explicit class attribute is not supported ##

Attribute Explicit when applied to a C# class is ignored by Translator. Translator processes a C# class marked with Explicit attribute as if the class was not marked with this attribute.

## NUnit's Ignore class attribute is not supported ##

Attribute Ignore when applied to a C# class is ignored by Translator. Translator processes a C# class marked with Ignore attribute as if the class was not marked with this attribute.

## C# method marked with NUnit's TestFixtureSetUp attribute is always translated into C++ static method ##

Text fixture method marked with TestFixtureSetUp attribute in C# language is always translated into a static method in C++ regardless of whether it was static or instance method of a C# class. It means that if an instance method in a C# class marked with TestFixtureSetUp attribute tries to access any non-static members of the class, such method will be translated into syntactically incorrect C++ method.

## C# method marked with NUnit's TestFixtureTearDown attribute is always translated into C++ static method ##

Text fixture method marked with TestFixtureTearDown attribute in C# language is always translated into a static method in C++ regardless of whether it was static or instance method of a C# class. It means that if an instance method in a C# class marked with TestFixtureTearDown attribute tries to access any non-static members of the class, such method will be translated into syntactically incorrect C++ method.

## Support of xUnit library is limited ##

xUnit library is not completely supported by Translator

## Compound assignment operators LHS is translated twice ##

LHS expression of compound assignment operators is translated twice. If it is a modifier expression, side effects are applied twice. Use `setter_wrap_with_lambda` option to bypass this limitation.
