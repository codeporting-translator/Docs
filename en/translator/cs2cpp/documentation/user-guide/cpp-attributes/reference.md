# C++ Attributes Reference #

This section describes attributes available in the CodePorting.Translator.Cs2Cpp.Control project. Use them to resolve translation issues or improve the translation experience.

[TOC]

> Note: Code examples used on this page are for illustration purposes only. Efforts were made to keep them as simple as possible. Actual translator output may differ.

## CodePorting.Translator Cs2Cpp attributes ##

These attributes are part of the translator itself and affect how the syntax node they're assigned to is translated. As a reminder, they can be assigned either directly in the C# code being translated or through [config files](../configuration-file/attributes.md).

### CppAddStructDefaultMethods ###

**Used on**: Structures

**Arguments**: None

Forces generating default ValueType methods: `operator==`, `Equals`, `ToString` and `GetHashCode`.

### CppAllowStackAllocation ###

**Used on**: Classes

**Arguments**: None

Allows translated classes to be allocated on stack by making their destructors public.

### CppArgumentKind ###

**Used on**: Function parameter

**Arguments**: Mandatory single value of `CodePorting.Translator.Cs2Cpp.ArgumentKind` enumeration.

Forces the use of a specific way to pass a parameter to a function (const reference, pointer, etc.). This option is useful mostly if you're overriding translated code with manually written code, as it only affects function formal parameters and not references to them. Calling contexts are not affected either.

Please note that C# semantics are followed by default, which means that pointer types are passed to functions by reference while value types are passed by value. You don't need to use this attribute to impose this behavior.

| Argument | Description | Usage example | Output code
| --- | --- | --- | ---
| Value | Value-type argument translation, same as if no CppArgumentKind attribute was used. | `void Foo([CppArgumentKind(ArgumentKind.Value)]Object value)` | `void Foo(System::SharedPtr<System::Object> value)`
| Reference | Reference function argument. | `void Foo([CppArgumentKind(ArgumentKind.Reference)]Object reference)` | `void Foo(System::SharedPtr<System::Object> &reference)`
| Pointer | Pointer function argument. | `void Foo([CppArgumentKind(ArgumentKind.Pointer)]Object pointer)` | `void Foo(System::Object *pointer)`
| ConstValue | Const value function argument. | `void Foo([CppArgumentKind(ArgumentKind.ConstValue)]Object constValue)` | `void Foo(System::SharedPtr<System::Object> const constValue)`
| ConstReference | Const reference function argument. | `void Foo([CppArgumentKind(ArgumentKind.ConstReference)]Object constReference)` | `void Foo(System::SharedPtr<System::Object> const &constReference)`
| ConstPointer | Const pointer function argument. | `void Foo([CppArgumentKind(ArgumentKind.ConstPointer)]Object constPointer)` | `void Foo(System::Object const *constPointer)`
| UnsafeConst | Mark unsafe pointer as const. | `unsafe void Foo([CppArgumentKind(ArgumentKind.UnsafeConst)]char *data)` | `void Foo(const char16_t *constPointer)`
| ArrayRawPointer | Make argument a pointer to collection data. | `unsafe void Foo([CppArgumentKind(ArgumentKind.ArrayRawPointer)]Object pointer)` | `void Foo(Object *constPointer)`
| ConstArrayRawPointer | Make argument a const pointer to collection data. | `unsafe void Foo([CppArgumentKind(ArgumentKind.ConstArrayRawPointer)]Object pointer)` | `void Foo(const Object *constPointer)`

### CppArrayInnerIndexer ###

**Used on**: Methods

**Arguments**: Names of the array variables used by this method

Makes the translator generate code that accesses the specified array's elements using the `std::vector` API rather than `System::Array` and `System::ArrayPtr`. This improves performance but removes boundary checking.

```cs
class ClassArrayInnerIndexer
{
    [CodePorting.Translator.Cs2Cpp.CppArrayInnerIndexer("array")]
    public void test_0()
    {
        int[] array = new int[] { 0, 1, 2, 3 };
        for(int n = 0; n < array.Length; ++n)
        {
            int k = array[n];
        }
    }
}
```

```cpp
void ClassArrayInnerIndexer::test_0()
{
    System::ArrayPtr<int32_t> array = System::MakeArray<int32_t>({0, 1, 2, 3});
    for (int32_t n = 0; n < array->get_Length(); ++n)
    {
        int32_t k = array->data().data()[n];
    }
}
```

**Since version**: 21.9

### CppArrayOnStack ###

**Used on**: Methods

**Arguments**: Names of the array variables to move to stack

Generates on-stack arrays in translated code instead of using System::Array wrappers. It can be applied only to single-dimensional arrays.

```cs
class ClassArrayOnStack
{
    [CodePorting.Translator.Cs2Cpp.CppArrayOnStack("arr1", "arr2", "arr3", "arr4", "arr6", "arr7", "arr8", "arr9")]
    public void ArrayOnStackMethod()
    {
        byte[] arr1 = new byte[] { 0 };
        byte[] arr2 = new byte[10];
        byte[,] arr3 = new byte[,] { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } }, arr4 = new byte[4, 2] { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } };
        byte[,] arr5 = new byte[,] { { 1, 2 }, { 3, 4 }, { 5, 6 }, { 7, 8 } };

        byte[,,] arr6 = new byte[,,] { { { 1, 2 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } };
        byte[,,] arr7 = new byte[2, 2, 2] { { { 1, 2 }, { 3, 4 } }, { { 5, 6 }, { 7, 8 } } };

        byte[,,] arr8 = new byte[,,] { { { 1, 2, 3 }, { 4, 5, 6 } }, { { 7, 8, 9 }, { 10, 11, 12 } } };
        byte[,,] arr9 = new byte[2, 2, 3] { { { 1, 2, 3 }, { 4, 5, 6 } }, { { 7, 8, 9 }, { 10, 11, 12 } } };
    }
}
```

```cpp
void ClassArrayOnStack::ArrayOnStackMethod()
{
    System::Details::StackArray<uint8_t, 1> arr1 = {0};
    System::Details::StackArray<uint8_t, 10> arr2 = {0};
    System::ArrayPtr<std::vector<uint8_t>> arr3 = System::MakeArray<std::vector<uint8_t>>({{1,2}, {3,4}, {5,6}, {7,8}}), arr4 = System::MakeArray<std::vector<uint8_t>>({{1,2}, {3,4}, {5,6}, {7,8}});
    System::ArrayPtr<std::vector<uint8_t>> arr5 = System::MakeArray<std::vector<uint8_t>>({{1,2}, {3,4}, {5,6}, {7,8}});
    
    System::ArrayPtr<std::vector<std::vector<uint8_t>>> arr6 = System::MakeArray<std::vector<std::vector<uint8_t>>>({{{1,2},{3,4}}, {{5,6},{7,8}}});
    System::ArrayPtr<std::vector<std::vector<uint8_t>>> arr7 = System::MakeArray<std::vector<std::vector<uint8_t>>>({{{1,2},{3,4}}, {{5,6},{7,8}}});
    
    System::ArrayPtr<std::vector<std::vector<uint8_t>>> arr8 = System::MakeArray<std::vector<std::vector<uint8_t>>>({{{1,2,3},{4,5,6}}, {{7,8,9},{10,11,12}}});
    System::ArrayPtr<std::vector<std::vector<uint8_t>>> arr9 = System::MakeArray<std::vector<std::vector<uint8_t>>>({{{1,2,3},{4,5,6}}, {{7,8,9},{10,11,12}}});
}
```

**Since version**: 21.9

### CppConstexpr ###

**Used on**: Field or class

**Arguments**: None

Forces translation of a field or of all fields of class as constexpr.

```cs
class CppConstexprTest
{
    [CodePorting.Translator.Cs2Cpp.CppConstexpr]
    static int i = 2 * 3;
}
```

```cpp
class CppConstexprTest : public System::Object, public ::testing::Test
{
    ...
    static constexpr int32_t i = 2 * 3;
};
```

### CppConstMethod ###

**Used on**: Method

**Arguments**: None

Adds 'const' modifier to method. Useful in situations where it is not generated by translator.

```cs
class Foo
{
    [CodePorting.Translator.Cs2Cpp.CppConstMethod]
    public void Bar() { ... }
}
```

```cpp
class Foo
{
    ...
    void Bar() const;
}
```

### CppConstRefParam ###

**Used on**: Method

**Argument**: Array of strings representing parameter names

Forces const reference form of function parameters in translated code. Same effect can be achieved using CppArgumentKind(ArgumentKind.ConstReference) attribute, but CppConstRefParam form is more compact. This parameter can be used e. g. to reduce shared pointer copying performance penalties.

```cs
class Foo
{
    [CodePorting.Translator.Cs2Cpp.CppConstRefParam("o")]
    public void Bar(Object o)
    { ... }
}
```

```cpp
class Foo
{
    ...
    void Bar(System::SharedPtr<System::Object> const &o);
};
```

### CppConstRefReturnType ###

**Used on**: Property, indexer, method

**Arguments**: None

Makes property/method return const references to avoid copying.

```cs
class Foo
{
    [CodePorting.Translator.Cs2Cpp.CppConstRefReturnType]
    public Foo Parent
    {
        get
        {
            return m_parent;
        }
    }
    private Foo m_parent;
}
```

```cpp
class Foo
{
    ...
    const SharedPtr<Foo>& get_Parent();
};
```

**Since version**: 21.7

### CppConstWrapper ###

**Used on**: Method

**Arguments**: None

Applies to non-const methods without parameters which override base class'es one. Creates a const overload of it which calls into non-const one using const_cast. Useful with e. g. non-const ToString() implementations, when there's a need to override a const method with a non-const implementation.

### CppCTORSelfReference ###

**Used on**: Constructor

**Arguments**: None

Generate guard which allows it to create temporary shared pointers during constructor execution without deleting the object. Not required if auto_ctor_self_reference option is enabled. Otherwise, creating shared pointers in constructor could lead to calling 'delete' on object being constructed.

### CppDeclareFriendClass ###

**Used on**: Class or interface

**Arguments**: Mandatory string argument with full name of friend type

Forces friend class declaration.

```cs
[CodePorting.Translator.Cs2Cpp.CppDeclareFriendClass("MyProject.Bar")]

class Foo
{
    private void Do() { ... }
}
```

```cpp
class Foo
{
    ...
    friend class MyProject::Bar;
};
```

### CppDeclareFriendFunction ###

**Used on**: Class

**Arguments**: Mandatory string argument with full C++-styled function declaration.

Forces friend function declaration. Also, forward declares the function to befriend.

```cs
[CodePorting.Translator.Cs2Cpp.CppDeclareFriendFunction("void Aspose::Function(int32_t value)")]
public class Bar
{
    public bool BoolBar()
    {
        return true;
    }
}
```

```cpp
namespace Aspose {  void Function(int32_t value); }

class Bar : public System::Object
{
    typedef Bar ThisType;
    typedef System::Object BaseType;
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    friend void Aspose::Function(int32_t value);
public:
    bool BoolBar();
};
```

**Since version**: 21.3

### CppDeferredInit ###

**Used on**: Class

**Arguments**: None (obsolete boolean argument may be used for compatibility reasons)

Forces all static variables in attributed class as singletons and static constructor to be called from static functions (including singleton accessors) and instance constructors rather than from C++ static variable initializer. Overrides effect of deferred_init option.

### CppDisableAutoReordering ###

**Used on**: Class or structure

**Arguments**: None

Makes translator put type members into C++ code in the same order they are in C# code, istead of grouping them by access modifier.

### CppEnumEnableMetadata ###

**Used on**: Enum types

**Arguments**: None

Forces metadata generation for enum (string representation of values for parsing and serializing). Use if you need conversions between enum values and strings in translated application. [cpp_enum_enable_metadata option](../configuration-file/options.md#cpp_enum_enable_metadata) enables this behavior globally.

### CppEnumWithOperators ###

**Used on**: Enum types

**Arguments**: None

Enables bitwise operators for enum. Use if you are using them.

### CppForceArrayInitializerCast ###

**Used on**: Methods

**Arguments**: None

Forces explicit casts of array initializers otherwise not working in C++ but allowed in C#.

```cs
[CodePorting.Translator.Cs2Cpp.CppForceArrayInitializerCast]
public void InitializeArrayFromDifferentType()
{
    int a = 1;
    int b = 2;
    float[] input_array = { a, b };
    Assert.AreEqual(2, input_array.Length);
}
```

```cpp
void ArrayTests::InitializeArrayFromDifferentType()
{
    int32_t a = 1;
    int32_t b = 2;
    System::ArrayPtr<float> input_array = System::MakeArray<float>(System::ObjectExt::ArrayInitializerCast<float>(a, b));
    ASPOSE_ASSERT_EQ(2, input_array->get_Length());
}
```

### CppForceForwardDeclaration ###

**Used on**: Classes

**Argument**: Mandatory string full name of type to forward declare.

Inside attributed type, always use forward declaration of argument type instead of include. Useful for loop type references management, etc.

### CppForceInclude ###

**Used on**: Classes

**Arguments**:

1. Mandatory string full name of type to include header for.
1. Optional boolean argument to force include even if suppressed by cross-includes lookup.
1. Boolean flag which will be removed in further versions of this attribute.

Inside attributed type, always include header with argument type definition instead of forward declaration.

### CppForceSharedApi ###

**Used on**: Entity

**Arguments**: None

Adds SHARED_API macro to entity declaration.

### CppForceStringParam ###

**Used on**: Function parameter

**Arguments**: None

Forces `String(...)` wrappers around string literals passed to attributed parameter. Useful if there is a problem with overloads (in C++, the priority converting string literals to boolean value is higher than to String class object). In C++, these calls are resolved used different rules from C#.

Since version 18.11, most cases like this get auto-resolved, so such attribute should only be used where translator analysis fails.

```cs
class Foo
{
    public static void Bar([CodePorting.Translator.Cs2Cpp.CppForceStringParam] String param) { ... }
    public static void AttributedTest(bool param) { ... }
}
...
Foo.Bar("abc");
```

```cpp
class Foo
{
    ...
    static void Bar(System::String param);
    static void Bar(bool param);
}
...
Foo::Bar(System::String(u"abc")); //If translated as 'Foo::Bar(u"abc")', this would call Foo::Bar(bool), not Foo::Bar(System::String&)
```

### CppFragment ###

**Used on**: Method

**Arguments**: C# code pattern to replace and C++ code to replace with.

Directly translates given C# code fragment as C++ code snippet.

The pattern is only applicable to statements, expressions, or variable declarations inside a method body. It can be raw C# source code or contain a single wildcard ('...' sign).

Replacement code can be:

* Raw C++ code to replace with.

```cs
[CodePorting.Translator.Cs2Cpp.CppFragment("10", "15")]
public void LiteralExpressionTest()
{
    var x = 10;
}
```

```cpp
void Fragment::LiteralExpressionTest()
{
    int32_t x = 15;
}
```

* C++ code with wildcard ('...' sign) to prefix or postfix normally translated code.

```cs
[CodePorting.Translator.Cs2Cpp.CppFragment("...= 10", "...0")]
public void PostfixTest()
{
    var x = 10;
}        
```

```cpp
void Fragment::PostfixTest()
{
    int32_t x = 100;
}
```

* C++ code with replacement ('-->' sign) to replace fragment in normally translated code.

```cs
[CodePorting.Translator.Cs2Cpp.CppFragment("if...", "11-->10")]
public void ReplacementTest()
{
    var x = 10;
    if (x == 11)
    {
        ++ x;
    }
}        
```

```cpp
void Fragment::ReplacementTest()
{
    int32_t x = 10;
    if (x == 10)
    {
        ++x;
    }
}
```

Replacement doesn't affect code semantics or add types to use. So if you want to use some undeclared C++ types here, you must add the [CppForceInclude](#cppforceinclude) attribute too.

### CppGenerateBeginEndMethods ###

**Used on**: Field or Auto-property

**Arguments**: None

Forces translator to generate begin/end methods that will delegate begin/end methods of a field or an auto-property.

The attribute triggering condition is that for the type of a field or auto-property to which the attribute is applied, the translator should generate begin/end methods, or this type should be one of the system collections that have begin/end methods.

This attribute has a lower priority than the CppNoBeginEndMethods attribute and a higher priority than the generate_begin_end_methods option.

```cs
public class Class0
{
    [CppGenerateBeginEndMethods]
    protected List<int> list;
}
```

```cpp
class Class0: public System::Object
{
    ...
public:
    /// A collection type whose iterator types is used as iterator types in the current collection.
    using iterator_holder_type = System::Collections::Generic::List<int32_t>;
    /// Iterator type.
    using iterator = typename iterator_holder_type::iterator;
    /// Const iterator type.
    using const_iterator = typename iterator_holder_type::const_iterator;
public:

    /// Gets iterator pointing to the first element (if any) of the collection.
    /// @return An iterator pointing to the first element (if any) of the collection
    iterator begin() noexcept;
    /// Gets iterator pointing right after the last element (if any) of the collection.
    /// @return An iterator pointing right after the last element (if any) of the collection
    iterator end() noexcept;
    /// Gets iterator pointing to the first element (if any) of the const-qualified instance of the collection.
    /// @return An iterator pointing to the first element (if any) of the const-qualified instance of the collection
    const_iterator begin() const noexcept;
    /// Gets iterator pointing right after the last element (if any) of the const-qualified instance of the collection.
    /// @return An iterator pointing right after the last element (if any) of the const-qualified instance of the collection
    const_iterator end() const noexcept;
    /// Gets iterator pointing to the first const-qualified element (if any) of the collection.
    /// @return An iterator pointing to the first const-qualified element (if any) of the collection
    const_iterator cbegin() const noexcept;
    /// Gets iterator pointing right after the last const-qualified element (if any) of the collection.
    /// @return An iterator pointing right after the last const-qualified element (if any) of the collection
    const_iterator cend() const noexcept;
    ...
};
```

### CppIgnoreBaseType ###

**Used on**: Type

**Argument**: Optional string type name to ignore inheritance from; defaults to `System.Object`

Omit inheritance from a specific type during translation. One usecase is creating a lightweight class not related to Object tree. If the type another than `System.Object` is ignored and there's no other references to `System.Object`, the translated class is still inherited from `System::Object`.

```cs
[CodePorting.Translator.Cs2Cpp.CppIgnoreBaseType("System.Object")]
interface Interface1 { }
interface Interface2 { }
[CodePorting.Translator.Cs2Cpp.CppIgnoreBaseType("Interface1")]
class Class1 : Interface1, Interface2 { }
[CodePorting.Translator.Cs2Cpp.CppIgnoreBaseType("Interface1")]
class class2 : Interface1 { }
```

```cpp
class Interface1 //No Object inheritance
{
    ...
};
class Interface2 : public virtual System::Object
{
    ...
};
class Class1 : public Interface2 //No interface1 inheritance
{
    ...
};
class Class2 : public System::Object
{
    ...
};
```

### CppIgnoreConstraints ###

**Used on**: Member or type

**Argument**: None

Disables translating 'where' clauses to C++.

### CppInline ###

**Used on**: Method, property accessor or type

**Argument**: Optional string comment which is always ignored

Moves an implementation of a member or of all members of a specific type to header file, even if not a template.

**Since version:** 20.11

### CppIOStreamWrapper ###

**Used on**: Method or constructor argument; property or indexer.

**Arguments**:

1. Mandatory: IOStreamType enumeration member which selects STL stream to use in the overload.
1. Optional: string name of template argument for the character type used by the stream.
1. Optional: string name of template argument for the character traits used by the stream.

Overloads the attributed entity with a version that accepts an STL stream instead of `SharedPtr<System::IO::Stream>`. The overload differs either by the attributed argument or by an implicit `value` argument.

```cs
public void IStream([CppIOStreamWrapper(IOStreamType.IStream)] Stream istream)
{
    ...
}
```

```cpp
void IStream(System::SharedPtr<System::IO::Stream> istream);
template <typename CharType, typename Traits = std::char_traits<CharType>>
void IStream(std::basic_istream<CharType, Traits>& istream)
{
    auto istreamWrapper = System::IO::WrapSTDIOStream(istream);
    IStream(istreamWrapper);
}
```

### CppLambdaPassByReference ###

**Used on**: Constructor, Method, or Property

**Arguments**: Local variable name

The specified local variable will be captured by lambda expressions by reference.

**Since version**: 21.2

### CppLambdaPassByValue ###

**Used on**: Constructor, Method, or Property

**Arguments**: Local variable name

Marks a method's local variable to be passed to a lambda function by value. Used to keep the passed value even when the local variable goes out of scope (for example, loop iterators).

### CppLambdaShouldCaptureByReference ###

**Used on**: Parameter

**Arguments**: Parameter name

The specified parameter will be captured by lambda expressions by reference when it is possible.

**Since version**: 21.2

### CppLambdaUseHolder ###

**Used on**: Constructor, Method, or Property

**Arguments**: Local variable name

The specified local variable will be wrapped into the `LambdaCaptureHolder` class instance. This class is used to synchronize the lifetime of the specified variable and a lambda expression that captures it.

**Since version**: 21.2

### CppMakeMembersPublic ###

**Used on**: Class or structure

**Arguments**: none

Makes all entities of the attributed type public in the translated code, regardless of their original scope.

### CppMutable ###

**Used on**: Field

**Arguments**: None

Marks field as mutable.

```cs
private class MutableHolder
{
    [CodePorting.Translator.Cs2Cpp.CppMutable]
    public int Mutable;
    public int NonMutable;
}
```

```cpp
class MutableHolder : public System::Object
{
    ...
    mutable int32_t Mutable;
    int32_t NonMutable;
};
```

### CppNightTest ###

**Used on**: Test fixture or test method

**Arguments**: None

Disables attributed test or all tests from attributed fixture unless `ASPOSE_ENABLE_NIGHT_TESTS` define is enabled. Useful to avoid running durable or unstable tests each time.

### CppNoBeginEndMethods ###

**Used on**: Class or Struct

**Arguments**: None

Prevents translator from generating begin/end methods.

This attribute has a higher priority than the CppGenerateBeginEndMethods attribute and generate_begin_end_methods option.

### CppNoConstMethod ###

**Used on**: Method

**Arguments**: None

Discards effect of [CppConstMethod](#cppconstmethod) attribute. Useful, if some methods are marked as const via config, but some overrides mustn't be const in C++.

**This attribute is legacy. It is likely to be removed in future versions of CodePorting.Translator Cs2Cpp.**

### CppOverrideAccessModifiers ###

**Used on**: Type or entity

**Arguments**: Mandatory value of AccessModifiers enum: Public, Internal, Protected or Private.

Switches the element's access modifier to the specified one in translated code. It also makes the translator behave as if the attributed entity had that specified access modifier, affecting all analysis performed.

**Since version:** 20.11

### CppOverrideTestFixtureSetUp ###

**Used on**: Method

**Arguments**: None

Forces translation of this method as `TestFixtureSetUp`, overriding any existing one. Also forces the method to be static.

### CppOverrideTestFixtureTearDowm ###

**Used on**: Method

**Arguments**: None

Forces translation of this method as `TestFixtureTearDown`, overriding any existing one. Also forces the method to be static.

### CppPlaceAfter ###

**Used on**: Type or member

**Arguments**: Mandatory string name of type or member to place attributed one after.

Moves attributed type or member to after the one named by the attribute parameter in output code.

```cs
[CodePorting.Translator.Cs2Cpp.CppPlaceAfter("Base")]
public class ClassWithInlineMethods
{
    public void SayHello()
    {
        Base.SayHello();
    }
}
public class Base
{

        [CodePorting.Translator.Cs2Cpp.CppPlaceAfter("Property")]
        public static void SayHello()
        {
            System.Console.WriteLine("Hello");
        }
        public int Property { get; set; }
        [CodePorting.Translator.Cs2Cpp.CppPlaceAfter("Field")]
        public Base() { }
        private int Field;
}
```

Translates into:

```cpp
class Base : public System::Object
{
private:
    int32_t pr_Property;
public:
    int32_t get_Property()
    {
        return pr_Property;
    }
    void set_Property(int32_t value)
    {
        pr_Property = value;
    }
    static void SayHello()
    {
        System::Console::WriteLine(u"Hello");
    }
private:
    int32_t Field;
public:
    Base() : pr_Property(0), Field(0)
    {
    }
};
class ClassWithInlineMethods : public System::Object
{
public:
    void SayHello()
    {
        Base::SayHello();
    }
};
```

**Since version:** 20.11

### CppPlaceBefore ###

**Used on**: Type or member

**Arguments**: Mandatory string name of type or member to place attributed one before.

Moves attributed type or member to before the one named by the attribute parameter in output code.

```cs
public class A
{ }
[CodePorting.Translator.Cs2Cpp.CppPlaceBefore("A")]
public class B
{ }
[CodePorting.Translator.Cs2Cpp.CppDisableAutoReordering]
public class ClassWithEnum
{
    public void Method(Enum e)
    {}
    public void Method()
    {}
    [CodePorting.Translator.Cs2Cpp.CppPlaceBefore("Method")]
    public enum Enum
    {
        A, B
    }
    [CodePorting.Translator.Cs2Cpp.CppPlaceBefore("Method")]
    public int Field;
}
```

Translates into:

```cpp
class B : public System::Object
{};
class A : public System::Object
{};
class ClassWithEnum : public System::Object
{
public:
    enum class Enum
    {
        A,
        B
    };
    int32_t Field;
    void Method(ClassWithEnum::Enum e);
    void Method();
    ClassWithEnum();
};
```

**Since version:** 20.11

### CppPortAsEnum ###

**Used on**: Integer const fields

**Arguments**: None

Translates numeric constant as enum (not enum class) with single value in it. Useful to define header-only constants (const fields are translated as static constants and require definition as an addition to header declaration).

```cs
class PortAsEnumTest
{
    [CodePorting.Translator.Cs2Cpp.CppPortAsEnum]
    const int EnumValue = 1;
    const int NonEnumValue = 1;
}
```

```cpp
class PortAsEnumTest
{
    ...
    enum{EnumValue = 1};
    static const int32_t NonEnumValue;
};
const int32_t PortAsEnumTest::NonEnumValue = 1;
```

### CppPortAsSingleton ###

**Used on**: Field

**Argument**: None; obsolete boolean argument may be used for compatibility reasons

Translates field as singleton. Same behavior can be enabled globally by the deferred_init option.

```cs
class SingletonOwner
{
    static System.Collections.Generic.List<string> staticList = new System.Collections.Generic.List<string>{ "abc", "def", "g", "hi" };
    [CodePorting.Translator.Cs2Cpp.CppPortAsSingleton]
    static System.Collections.Generic.List<string> singletonList = new System.Collections.Generic.List<string>{ "abc", "def", "g", "hi" };
}
```

```cpp
class SingletonOwner
{
    ...
    static System::SharedPtr<System::Collections::Generic::List<System::String>> staticList;
    static System::SharedPtr<System::Collections::Generic::List<System::String>>& singletonList();
    static struct __StaticConstructor__ { __StaticConstructor__(); } s_constructor__;
};
SingletonOwner::__StaticConstructor__ SingletonOwner::s_constructor__;
System::SharedPtr<System::Collections::Generic::List<System::String>> SingletonOwner::staticList;
SingletonOwner::__StaticConstructor__::__StaticConstructor__()
{
    staticList = [&]{ System::String init_3[] = {u"abc", u"def", u"g", u"hi"}; auto list_3 = System::MakeObject<System::Collections::Generic::List<System::String>>(); list_3->AddInitializer(4, init_3); return list_3; }();
}
System::SharedPtr<System::Collections::Generic::List<System::String>>& SingletonOwner::singletonList()
{
    static System::SharedPtr<System::Collections::Generic::List<System::String>> value;
    static std::once_flag once;
    std::call_once(once, []
    {
        value = [&]{ System::String init_0[] = {u"abc", u"def", u"g", u"hi"}; auto list_0 = System::MakeObject<System::Collections::Generic::List<System::String>>(); list_0->AddInitializer(4, init_0); return list_0; }();
    });
    return value;
}
```

### CppPortConstStringAsWChar ###

**Used on**: Field or class

**Arguments**: None

Translates attributed string field or all string fields in attributed class as `const char16_t*` instead of `System::String`. Only possible for const or readonly fields with plain initializers. Resolves initialization race issues, speeds up startup.

### CppPreMaterialize ###

**Used on**: Method

**Arguments**: None

Tells translator to pre-evaluate all yield members and return result as list.

### CppRenameEntity ###

**Used on**: Method or property

**Argument**: Optional string with new name. If absent, old name with '_' prefix or suffix will be used, depending on context.

Renames method or property in output code. Useful if you have name clash which can't be resolved on C++ side (e. g. two virtual methods with same signatures or C++ keyword used as an identifier). Affects both symbol and references to it.

```cs
[CodePorting.Translator.Cs2Cpp.CppRenameEntity("NewName")]
public String OldName()
{
    return "ClassName.OldName";
}
```

```cpp
System::String ClassName::NewName()
{
    return u"ClassName.OldName";
}
```

### CppSelfReference ###

**Used on**: Method

**Arguments**: None

Same as [CppCTORSelfReference](#cppctorselfreference), but works on method. No auto-placement option at the moment exists for this attribute. Useful if method is called when no references to object exist (e. g. during construction or destruction).

### CppSkipDefinition ###

**Used on**: Class, struct, method

**Argument**: Optional boolean value - whether to generate exception-throwing stubs; defaults to 'true'.

Skips translating method or class definition, leaving declaration in place. Optionally, generates stubs that throw exception without actually doing anything. Use this attribute on methods you want to substitute native C++ implementation to.

```cs
class SkipDefinitionTest
{
    [CodePorting.Translator.Cs2Cpp.CppSkipDefinition]
    private static String Skip1()
    {
        return "C#";
    }
    [CodePorting.Translator.Cs2Cpp.CppSkipDefinition(true)]
    private static String Skip2()
    {
        return "C#";
    }
    [CodePorting.Translator.Cs2Cpp.CppSkipDefinition(false)]
    private static String Skip3()
    {
        return "C#";
    }
}
```

```cpp
class SkipDefinitionTest
{
    ...
    static System::String Skip1();
    static System::String Skip2();
    static System::String Skip3();
};
System::String SkipDefinitionTest::Skip1() { throw System::NotImplementedException(ASPOSE_CURRENT_FUNCTION); }
System::String SkipDefinitionTest::Skip2() { throw System::NotImplementedException(ASPOSE_CURRENT_FUNCTION); }
//No body generated for Skip3
```

### CppSkipEntity ###

**Used on**: Entity

**Argument**: Optional comment string. Always ignored.

Skips class, struct, enum, method, field, etc. The output code looks as if the entity was not present in the input at all. Unlike [CppSkipDefinition](#cppskipdefinition), skips both declarations and definitions of the affected entities. Note that any references to skipped entity in translated code end up in compilation errors.

### CppSkipTest ###

**Used on**: Method.

**Argument**: None.

Adds a `GTEST_SKIP` call at the beginning of the method. Does not apply to non-test methods.

### CppStaticMethod ###

**Used on**: Method

**Argument**: None

Forces method which is otherwise non-static to be translated as static.

### CppStaticVariable ###

**Used on**: Method

**Argument**: String name of variable inside attributed function

Forces argument variable as static. Affects all variables with same name. Only affects declarations containing single variable per declaration. Useful to cache large objects (e. g. string arrays, etc.).

```cs
[CodePorting.Translator.Cs2Cpp.CppStaticVariable("colors")]
private string GetFruitColor(string fruit)
{
    System.Collections.Generic.Dictionary<string, string> colors = new System.Collections.Generic.Dictionary<string, string> { { "banana", "yellow" }, { "apple", "red" }, { "carrot", "orange" } };
    return colors[fruit];
}
```

### CppUseAlternativeSwitch ###

**Used on**: Method

**Arguments**: None

Forces using do-while form of switch translation inside this method. Similar to [alternative_string_switch option](../configuration-file/options.md#alternative_string_switch), but only affects single method.

### CppUseReflection ###

**Used on**: Class

**Arguments**: None

### CppUseWeakPtrToCaptureThis ###

**Used on**: Class, Constructor, Method, or Property

**Arguments**: None

Used to capture `this` using the `self` variable that stores a `WeakPtr` smart pointer to the current object. This applies to lambda translation.

### CppUsing ###

**Used on**: Class, interface

**Arguments**:

1. Mandatory string parameter: method name
1. Optional `CodePorting.Translator.Cs2Cpp.CppUsingAttribute.Visibility` enum parameter - closure level (defaults to public)

Generates 'using' directives that import symbols from base classes.

```cs
class UsingTestParent
{
    protected void Foo(bool b)
    { }
}
[CodePorting.Translator.Cs2Cpp.CppUsing("UsingTestBase::Foo")]
class UsingTestChild : UsingTestParent
{}
```

```cpp
class UsingTestParent: public System::Object
{
    ...
protected:
    void Foo(bool b);
};
class UsingTestChild : public UsingTestMiddle
{
    ...
public:
    using UsingTestMiddle::UsingTestBase::Foo;
};
```

### CppVirtualInheritance ###

**Used on**: Class, interface

**Argument**: Name of the base class or interface to inherit from virtually (mandatory as there can be multiple base classes)

Forces virtual inheritance instead of direct.

### CppWeakPtr ###

**Used on**: Field

**Argument**:

1. Without parameters affects variable itself.
1. Single integer parameter denotes index of generic argument which should be passed as weak pointer rather than shared pointer.

Force holding value as weak pointer rather than shared pointer.

```cs
class WeakPtrTest_Cs
{
    public Object s;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr]
    public Object w;

    public List<Object> ss;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr]
    public List<Object> ws;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr(0)]
    public List<Object> sw;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr(0), CodePorting.Translator.Cs2Cpp.CppWeakPtr]
    public List<Object> ww;

    public class TwoArgs<A,B>
    {
        public A a;
        public B b;
    }

    public TwoArgs<Object,Object> sss;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr]
    public TwoArgs<Object,Object> wss;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr(0)]
    public TwoArgs<Object,Object> sws;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr(1)]
    public TwoArgs<Object,Object> ssw;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr(0), CodePorting.Translator.Cs2Cpp.CppWeakPtr(1)]
    public TwoArgs<Object,Object> sww;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr, CodePorting.Translator.Cs2Cpp.CppWeakPtr(0)]
    public TwoArgs<Object,Object> wws;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr, CodePorting.Translator.Cs2Cpp.CppWeakPtr(1)]
    public TwoArgs<Object,Object> wsw;
    [CodePorting.Translator.Cs2Cpp.CppWeakPtr, CodePorting.Translator.Cs2Cpp.CppWeakPtr(0), CodePorting.Translator.Cs2Cpp.CppWeakPtr(1)]
    public TwoArgs<Object,Object> www;
}
```

```cpp
class WeakPtrTest_Cs
{
    ...
public:
    template<typename A, typename B>
    class TwoArgs
    {
        ...
    public:
        A a;
        B b;
    };

    System::SharedPtr<System::Object> s;
    System::WeakPtr<System::Object> w;
    System::SharedPtr<System::Collections::Generic::List<System::SharedPtr<System::Object>>> ss;
    System::WeakPtr<System::Collections::Generic::List<System::SharedPtr<System::Object>>> ws;
    System::DynamicWeakPtr<System::Collections::Generic::List<System::SharedPtr<System::Object>>, System::SmartPtrMode::Shared, 0> sw;
    System::DynamicWeakPtr<System::Collections::Generic::List<System::SharedPtr<System::Object>>, System::SmartPtrMode::Weak, 0> ww;
    System::SharedPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>> sss;
    System::WeakPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>> wss;
    System::DynamicWeakPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>, System::SmartPtrMode::Shared, 0> sws;
    System::DynamicWeakPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>, System::SmartPtrMode::Shared, 1> ssw;
    System::DynamicWeakPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>, System::SmartPtrMode::Shared, 0, 1> sww;
    System::DynamicWeakPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>, System::SmartPtrMode::Weak, 0> wws;
    System::DynamicWeakPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>, System::SmartPtrMode::Weak, 1> wsw;
    System::DynamicWeakPtr<System::Collections::Generic::Dictionary<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>, System::SmartPtrMode::Weak, 0, 1> www;
};
```

### CppWrapInMacro ###

**Used on**: Method

**Argument**: Mandatory string expression to be tested as an argument of `#if` statement

Wrap method in macro.

```cs
private class FooContainer
{
    [CodePorting.Translator.Cs2Cpp.CppWrapInMacro("defined(FOO_FUNCTION_EXISTS)")]
    public static void Foo()
    {}
}
```

```cpp
class FooContainer
{
    ...
    #if defined(FOO_FUNCTION_EXISTS)
        static void Foo();
    #endif
}
```

## .NET attributes ##

This section lists .NET attributes recognized by the translation application.

### System.Diagnostics.Conditional ###

**Used on**: Methods

**Argument**: Mandatory string name of preprocessor definition

Wraps method body with `#if defined` directives with attribute argument as definition name. Multiple attributes join using 'logical or' coupling.

```cs
[System.Diagnostics.Conditional("TEST_DEF")]
public void func1(int val)
{
    func3(val);
}
[Conditional("NONEXISTENT"), ConditionalAttribute("TEST_DEF"), System.Diagnostics.Conditional("BOO")]
public void func2(int val)
{
    func3(val);
}
```

```cpp
void ClassName::func1(int32_t val)
{
#if defined(TEST_DEF)
    func3(val);
#endif
}
void ClassName::func2(int32_t val)
{
#if defined(NONEXISTENT) || defined(TEST_DEF) || defined(BOO)
    func3(val);
#endif
}
```

### System.ThreadStatic ###

**Used on**: static fields

**Arguments**: None

Puts static variable into thread scope. In C++, translates into 'thread_local' scope specifier.

```cs
class MyThread
{
    public int value0;
    [ThreadStatic]
    public static int value1 = 0;
    [ThreadStatic]
    public int value2 = 0;
    public static int value3 = 0;
}
```

```cpp
class MyThread
{
    ...
    int32_t value0;
    static thread_local int32_t value1;
    int32_t value2;
    static int32_t value3;
};
thread_local int32_t MyThread::value1 = 0;
int32_t MyThread::value3 = 0;
```

### System.Obsolete ###

**Used on**: classes and members

**Arguments**: None

**Supported since version**: 19.11

Translates to `@deprecated` Doxygen annotation.

## NUnit attributes ##

This section lists supported attributes from the NUnit framework. For more information on how to use these attributes, please refer to the NUnit manual.

The below example shows usage of some NUnit attributes supported by CodePorting.Translator from C# to C++.

```cs
using System;
using NUnit.Framework;
[TestFixture]
class TestWithArguments
{
    [Test]
    public void TestWithMultipleArguments([Values("a", "b", "c")] string str1, [Values("a", "d", "e")] string str2)
    {
        Assert.IsTrue(str1 == str2 || str1 != str2);
    }
}
```

For more information, please refer to NUnit site at [https://nunit.org/](https://nunit.org/).

### NUnit.Framework.Category ###

**Used on**: TestFixture class methods

Maps test methods into categories allowing translator excluding them based on category name.

### NUnit.Framework.ExpectedException ###

**Used on**: TestFixture class methods

Marks test method with expected exception information.

### NUnit.Framework.Explicit ###

**Used on**: TestFixture class methods

Marks method as explicit test.

### NUnit.Framework.Ignore ###

**Used on**: TestFixture class methods

Ignores test (by adding 'DISABLED_' prefix to C++ test name).

### NUnit.Framework.OneTimeSetUp ###

**Used on**: TestFixture class methods

Marks the method as a test fixture setup method.

### NUnit.Framework.OneTimeTearDown ###

**Used on**: TestFixture class methods

Marks the method as a test fixture teardown method.

### NUnit.Framework.SetCulture ###

**Used on**: TestFixture class methods

Forces using specific culture when running test method.

### NUnit.Framework.SetUp ###

**Used on**: TestFixture class methods

Marks the method as a test setup method.

### NUnit.Framework.Sequential ###

**Used on**: TestFixture class methods

Marks test as sequential.

### NUnit.Framework.TearDown ###

**Used on**: TestFixture class methods

Marks the method as a test teardown method.

### NUnit.Framework.Test ###

**Used on**: TestFixture class methods

Marks method as a test,

### NUnit.Framework.TestCase ###

**Used on**: TestFixture class methods

Adds test case based on attributed method.

### NUnit.Framework.TestCaseSource ###

**Used on**: TestFixture class methods

Specifies test case.

### NUnit.Framework.TestFixture ###

**Used on**: Classes

**Arguments**: None

Marks class as test fixture.

### NUnit.Framework.TestFixtureSetUp ###

**Used on**: TestFixture class methods

Marks the method as a test fixture setup method.

### NUnit.Framework.TestFixtureTearDown ###

**Used on**: TestFixture class methods

Marks the method as a test fixture teardown method.

### NUnit.Framework.Timeout ###

**Used on**: TestFixture class methods

Sets up timeout for test (e. g. for performance testing).

### NUnit.Framework.Repeat ###

**Used on**: TestFixture class methods

Runs the test method a given number of times. Any iteration failure causes the entire test to fail.

### NUnit.Framewor.Values ###

**Used on**: Test method arguments

Specifies values to run test with.

## xUnit framework attributes ##

This section lists supported xUnit framework attributes. For more information please refer to the xUnit site at [https://github.com/xunit/xunit](https://github.com/xunit/xunit).

### Xunit.Fact ###

**Used on**: Methods

Marks method as fact.

### Xunit.InlineData ###

**Used on**: Methods

Specifies theory inline data.

### Xunit.Theory ###

**Used on**: Methods

Marks method as theory.

## Obsolete attributes ##

This section describes attributes available in the CodePorting.Translator.Cs2Cpp.Control project that are obsolete and no longer meaningful. Do not use them and remove them from your C# code as soon as possible.

### CppAllowBoxing ###

**Used on**: Structures

**Arguments**: None

**Obsolete since version**: 26.8

Allows boxing for this type. This requires the type to implement operator == (), ToString() const and GetHashCode() const.

### CppDisableEnumeratorCurrentValueHolder ###

**Used on**: Class or structure

**Arguments**: None

Disables value holding in a particular enumerator class.

**Obsolete since version**: 26.8

### CppDoNotObfuscate ###

**Used on**: Entity

**Arguments**: None

Disables entity obfuscation.

**Obsolete since version**: 26.8

### CppEmitEnumeratorCurrentValueHolder ###

**Used on**: Class or structure

**Arguments**: None

Emits value holding in a particular enumerator class.

**Obsolete since version**: 26.8

### CppExactArrayInitializer ###

**Used on**: Array type field

**Arguments**: None

Passes array initializer expression directly from C# to C++ without parsing and code generation. Speeds up long initializers (like thousands of string elements, etc.).

**Obsolete since version**: 26.8

### CppForceDynamicCastFromTypeParam ###

**Used on**: Generic type or method

**Argument**: String name of template parameter to affect

Forces dynamic casts from argument type parameter to Object instead of static casts used by default.

**Obsolete since version**: 22.9

### CppForceDynamicCastToTypeParam ###

**Used on**: Generic type or method

**Argument**: String name of template parameter to affect

Forces dynamic casts from Object to argument type parameter instead of static casts used by default.

**Obsolete since version**: 22.9

### CppForceObfuscate ###

**Used on**: Entities

**Arguments**: None

Forces attributed entity to be obfuscated if 'obfuscate_cpp_headers' option is enabled.

**Obsolete since version**: 26.8

### CppNoAbstract ###

**Used on**: Class

**Arguments**: None

Omit 'abstract' mark when translating the class.

**Obsolete since version**: 26.8

### CppOverride ###

**Used on**: Method

**Arguments**: None

Makes translator mark method with 'override' qualifier.

**Obsolete since version**: 26.8

### CppUnknownTypeParam ###

**Used on**: Generic type or method

**Argument**: String name of template parameter to treat as unknown type

Force treating type argument as unknown type: calling ObjectExt::UnknownToObject() and ObjectExt::ObjectToUnknown() whenever the conversion is required. Fixes some type conversion issues with type parameters.

**Obsolete since version**: 22.9

### CppValueTypeParam ###

**Used on**: Generic type or method

**Argument**: String name of template parameter to treat as value type

Force treating type argument as value type: boxing, etc.

**Obsolete since version**: 26.8
