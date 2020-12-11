---
date: "2020-03-28"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp Attributes"
linktitle: "CodePorting.Native Cs2Cpp Attributes"
menu:
  docs:
    parent: "Developer Guide"
    weight: "5"
lastmod: "2020-03-28"
weight: "5"
---

## Overview ##

There is a number of attributes recognized by CodePorting.Native Cs2Cpp. __These include both widely-known attributes introduced by .NET or third party libraries such as NUnit and special attributes introduced by CodePorting.Native Cs2Cpp__ itself. The former attributes can be used in the same way as they are used normally in C# applications. The later can be placed into source code manually to prepare the code for porting; they are defined in CsToCppPorter namespace. This page summarizes their effects.

To use an attribute, you must make it visible from your C# code. To do so, you should reference CsToCppPorter.Attributes project from the one you are preparing to port. Please note that the porting application itself doesn't analyze attribute definitions as they are recognized by names; therefore, inheriting attributes to simplify constructor arguments or for some other reasons won't work. Please check more details as following.

{{<note>}}
Please note code examples used on this page are for illustration purposes only. Efforts were put to keep them as simple as possible. Actual porting application output may differ.
{{</note>}}


## CodePorting.Native Cs2Cpp private attributes ##

This section describes attributes available in CsToCppPorter.Attributes project. Use them to resolve porting issues or to improve porting experience.

### CppAddStructDefaultMethods ###

**Used on**: Structures
**Arguments**: None

Forces generating default ValueType methods: operator##, Equals, ToString and GetHashCode.

### Cpp AllowBoxing ###

**Used on**: Structures
**Arguments**: None

Allows boxing for this type. This requires the type to implement operator ## (), ToString() const and GetHashCode() const.

### CppArgumentKind ###

**Used on**: Function parameter
**Arguments**: Mandatory single value of CsToCppPorter.ArgumentKind enumeration.

Forces to use a specific way to pass a parameter to function (const reference, pointer, etc.). This option is useful mostly if you're overriding your ported code with manually written one, as it only affects function formal parameters but not the references to them. Calling contexts are not affected as well.

Please note that C# semantics is followed by default which means that pointer types are passed to functions by reference while value types are passed by value. You don't need to use this attribute to impose this behavior.

| Argument |# Description |# Usage example |# Output code
---| ---|  ---|  ---|
| Value | Value-type argument translation, same as if no CppArgumentKind attribute was used. | {{<highlight cs >}}
void Foo([CsToCppPorter.CppArgumentKind(CsToCppPorter.ArgumentKind.Value)]Object value) { ... }
{{< /highlight >}} | {{< highlight cpp >}}
void Foo(System::SharedPtr<System::Object> value) { ... }
{{< /highlight >}}
| Reference | Reference function argument. | {{< highlight cs >}}
void Foo([CsToCppPorter.CppArgumentKind(CsToCppPorter.ArgumentKind.Reference)]Object reference) { ... }
{{< /highlight >}} | {{< highlight cpp >}}
void Foo(System::SharedPtr<System::Object> &reference) { ... }
{{< /highlight >}}
| Pointer | Pointer function argument. | {{< highlight cs >}}
void Foo([CsToCppPorter.CppArgumentKind(CsToCppPorter.ArgumentKind.Pointer)]Object pointer) { ... }
{{< /highlight >}} | {{< highlight cpp >}}
void Foo(System::Object *pointer) { ... }
{{< /highlight >}}
| ConstValue | Const value function argument. | {{< highlight cs >}}
void Foo([CsToCppPorter.CppArgumentKind(CsToCppPorter.ArgumentKind.ConstValue)]Object constValue) { ... }
{{< /highlight >}} | {{< highlight cpp >}}
void Foo(System::SharedPtr<System::Object> const constValue) { ... }
{{< /highlight >}}
| ConstReference | Const reference function argument. | {{< highlight cs >}}
void Foo([CsToCppPorter.CppArgumentKind(CsToCppPorter.ArgumentKind.ConstReference)]Object constReference) { ... }
{{< /highlight >}} | {{< highlight cpp >}}
void Foo(System::SharedPtr<System::Object> const &constReference) { ... }
{{< /highlight >}}
| ConstPointer | Const pointer function argument. | {{< highlight cs >}}
void Foo([CsToCppPorter.CppArgumentKind(CsToCppPorter.ArgumentKind.ConstPointer)]Object constPointer) { ... }
{{< /highlight >}} | {{< highlight cpp >}}
void Foo(System::Object const *constPointer) { ... }
{{< /highlight >}}
### CppConstexpr ###

**Used on**: Field or class
**Arguments**: None

Forces translation of a field or of all fields of class as constexpr.

{{< highlight cs >}}
class CppConstexprTest
{
    [CsToCppPorter.CppConstexpr]
    static int i = 2 * 3;
}
{{< /highlight >}}

{{< highlight cpp >}}
class CppConstexprTest : public System::Object, public ::testing::Test
{
    ...
    static constexpr int32_t i = 2 * 3;
};
{{< /highlight >}}

### CppConstMethod ###

**Used on**: Method
**Arguments**: Optional boolean argument to generate 'ASPOSE_CONST' marker instead of 'const' one.

Adds 'const' modifier to method. Useful in situations where it is not generated by porter.

| Argument |# Description |# Usage example |# Output code
---| ---|  ---|  ---|
| No CppConstMethod attribute | Method is translated as non-const. | {{< highlight cs >}}
class Foo
{
    public void Bar() { ... }
}
{{< /highlight >}} |{{< highlight cpp >}}
class Foo
{
    ...
    void Bar();
}
{{< /highlight >}}  
| No argument | Method is translated as const. | {{< highlight cs >}}
class Foo
{
    [CsToCppPorter.CppConstMethod]
    public void Bar() { ... }
}
{{< /highlight >}} | {{< highlight cpp >}}
class Foo
{
    ...
    void Bar() const;
}
{{< /highlight >}}
| false | Method is translated as const, same as without argument. | {{< highlight cs >}}
class Foo
{
    [CsToCppPorter.CppConstMethod(false)]
    public void Bar() { ... }
}
{{< /highlight >}}  | {{< highlight cpp >}}
class Foo
{
    ...
    void Bar() const;
}
{{< /highlight >}}
| true | Method is translated as ASPOSE_CONST, useful if you're inheriting from ASPOSE_CONST method and use library built with ASPOSE_NO_CONST_METHODS cmake option enabled. | {{< highlight cs >}}
class Foo
{
    [CsToCppPorter.CppConstMethod(true)]
    public void Bar() { ... }
}
{{< /highlight >}} | {{< highlight cpp >}}
class Foo
{
    ...
    void Bar ASPOSE_CONST();
}
{{< /highlight >}}

### CppConstRefParam ###

**Used on**: Method
**Argument**: Array of strings representing parameter names

Forces const reference form of function parameters in translated code. Same effect can be achieved using CppArgumentKind(ArgumentKind.ConstReference) attribute, but CppConstRefParam form is more compact. This parameter can be used e. g. to reduce shared pointer copying performance penalties.

{{< highlight cs >}}
class Foo
{
    [CsToCppPorter.CppConstRefParam("o")]
    public void Bar(Object o)
    { ... }
}
{{< /highlight >}}

{{< highlight cpp >}}
class Foo
{
    ...
    void Bar(System::SharedPtr<System::Object> const &o);
};
{{< /highlight >}}

### CppConstWrapper ###

**Used on**: Method
**Arguments**: None

Applies to non-const methods without parameters which override base class'es one. Creates a const overload of it which calls into non-const one using const_cast. Useful with e. g. non-const ToString() implementations, when there's a need to override a const method with a non-const implementation.

### CppCTORSelfReference ###

**Used on**: Constructor
**Arguments**: None

Generate guard which allows it to create temporary shared pointers during constructor execution without deleting the object. Not required if the auto_ctor_self_reference option is enabled. Otherwise, creating shared pointers in constructor could lead to calling 'delete' on the object being constructed.

### CppDeclareFriendClass ###

**Used on**: Class or interface
**Arguments**: Mandatory string argument with full name of friend type

Forces friend class declaration.

{{< highlight cs >}}
[CsToCppPorter.CppDeclareFriendClass("MyProject.Bar")]
class Foo
{
    private void Do() { ... }
}
{{< /highlight >}}

{{< highlight cpp >}}
class Foo
{
    ...
    friend class MyProject::Bar;
};
{{< /highlight >}}

### CppDeferredInit ###

**Used on**: Class
**Arguments**: None (obsolete boolean argument may be used for compatibility reasons)

Forces all static variables in attributed class as singletons and static constructor to be called from static functions (including singleton accessors) and instance constructors rather than from C++ static variable initializer. Overrides effect of deferred_init option.

### CppDisableAutoReordering ###

**Used on**: Class or structure
**Arguments**: None

Makes porter put type members into C++ code in the same order they are in C# code, istead of grouping them by access modifier.

### CppEnumEnableMetadata ###

**Used on**: Enum types
**Arguments**: None

Forces metadata generation for enum (string representation of values for parsing and serializing). Use if you need conversions between enum values and strings in ported application. [cpp_enum_enable_metadata option](https://docs.codeporting.com/native/cs2cpp/Developer-Guide/2.6-CodePorting.Native-Cs2Cpp-Configuration-File/Configuration-file-Options/#Hcpp_enum_enable_metadata) enables this behavior globally.

### CppEnumWithOperators ###

**Used on**: Enum types
**Arguments**: None

Enables bitwise operators for enum. Use if you are using them.

### CppExactArrayInitializer ###

**Used on**: Array type field
**Arguments**: None

Passes array initializer expression directly from C# to C++ without parsing and code generation. Speeds up long initializers (like thousands of string elements, etc.).

### CppForceDynamicCastFromTypeParam ###

**Used on**: Generic type or method
**Argument**: String name of template parameter to affect

Forces dynamic casts from argument type parameter to Object instead of static casts used by default.

### CppForceDynamicCastToTypeParam ###

**Used on**: Generic type or method
**Argument**: String name of template parameter to affect

Forces dynamic casts from Object to argument type parameter instead of static casts used by default.

### CppForceArrayInitializerCast ###

**Used on**: Methods
**Arguments**: None

Forces explicit casts of array initializers otherwise not working in C++ but allowed in C#.

{{< highlight cs >}}
[CsToCppPorter.CppForceArrayInitializerCast]
public void InitializeArrayFromDifferentType()
{
    int a = 1;
    int b = 2;
    float[] input_array = { a, b };
    Assert.AreEqual(2, input_array.Length);
}
{{< /highlight >}}

{{< highlight cpp >}}
void ArrayTests::InitializeArrayFromDifferentType()
{
    int32_t a = 1;
    int32_t b = 2;
    System::ArrayPtr<float> input_array = System::MakeArray<float>(System::ObjectExt::ArrayInitializerCast<float>(a, b));
    ASPOSE_ASSERT_EQ(2, input_array->get_Length());
}
{{< /highlight >}}

### CppForceForwardDeclaration ###

**Used on**: Classes
**Argument**: Mandatory string full name of type to forward declare

Inside attributed type, always use forward declaration of argument type instead of include. Useful for loop type references management, etc.

### CppForceInclude ###

**Used on**: Classes
**Arguments**:

1. Mandatory string full name of type to include header for.
1. Optional boolean argument to force include even if no references to argument type exists, defaults to false.

Inside attributed type, always include header with argument type definition instead of forward declaration.

### CppForceSharedApi ###

**Used on**: Entity
**Arguments**: None

Adds SHARED_API macro to entity declaration.

### CppForceStringParam ###

**Used on**: Function parameter
**Arguments**: None

Forces 'String(...)' wrappers around string literals passed to attributed parameter. Useful if there is a problem with overloads (in C++, __the priority converting string literals to boolean value is higher than to String class object.__ In C++, these calls are resolved used different rules from C#.

Since version 18.11, most cases like this get auto-resolved, so such attribute should only be used where porter analysis fails.

{{< highlight cs >}}
class Foo
{
    public static void Bar([CsToCppPorter.CppForceStringParam] String param) { ... }
    public static void AttributedTest(bool param) { ... }
}
...
Foo.Bar("abc");
{{< /highlight >}}

{{< highlight cpp >}}
class Foo
{
    ...
    static void Bar(System::String param);
    static void Bar(bool param);
}
...
Foo::Bar(System::String(u"abc")); //If translated as 'Foo::Bar(u"abc")', this would call Foo::Bar(bool), not Foo::Bar(System::String&)
{{< /highlight >}}

### CppIgnoreBaseType ###

**Used on**: Type
**Argument**: Optional string type name to ignore inheritance from; defaults to System.Object

Omit inheritance from a specific type during translation. One usecase is creating a lightweight class not related to Object tree. If the type another than System.Object is ignored and there's no other references to System.Object, the ported class is still inherited from System::Object.

{{< highlight cs >}}
[CsToCppPorter.CppIgnoreBaseType("System.Object")]
interface Interface1 { }
interface Interface2 { }
[CsToCppPorter.CppIgnoreBaseType("Interface1")]
class Class1 : Interface1, Interface2 { }
[CsToCppPorter.CppIgnoreBaseType("Interface1")]
class class2 : Interface1 { }
{{< /highlight >}}

{{< highlight cpp >}}
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
{{< /highlight >}}

### CppIgnoreConstraints ###

**Used on**: Method or type
**Argument**: Optional string comment which is always ignored

Disables translating 'where' clauses to C++.

### CppInline ###

**Used on**: Member or type
**Argument**: None

Moves an implementation of a member or of all members of a specific type to header file, even if not a template.

**Since version:** 20.11

### CppLambdaPassByValue ###

**Used on**: Method
**Arguments**: Local variable name

Mark method's local variable to pass to lambda-function by value. Used to keep passed value, even in case, when local variable is out of scope (i.e. loop iterators, and so on)

### CppMutable ###

**Used on**: Field
**Arguments**: None

Marks field as mutable.

{{< highlight cs >}}
private class MutableHolder
{
    [CsToCppPorter.CppMutable]
    public int Mutable;
    public int NonMutable;
}
{{< /highlight >}}

{{< highlight cpp >}}
class MutableHolder : public System::Object
{
    ...
    mutable int32_t Mutable;
    int32_t NonMutable;
};
{{< /highlight >}}

### CppNightTest ###

**Used on**: Test fixture or test method
**Arguments**: None

Disables attributed test or all tests from attributed fixture unless ASPOSE_ENABLE_NIGHT_TESTS define is enabled. Useful to avoid running durable or unstable tests each time.

### CppNoAbstract ###

**Used on**: Class
**Arguments**: None

Omit 'abstract' mark when translating the class.

### CppConstMethod ###

**Used on**: Method
**Arguments**: None

Discards the effect of the 'CppConstMethod' attribute. Useful, if some methods are marked as const via config, but some overrides mustn't be const in C++.

**This attribute is legacy. It is likely to be removed in future versions of CodePorting.Native Cs2Cpp.**

### CppOverride ###

**Used on**: Method
**Arguments**: None

Makes porter mark method with 'override' qualifier.

### CppOverrideAccessModifiers ###
**Used on**: Type or entity
**Arguments**: Mandatory value of AccessModifiers enum

Switches element's access modifier to specified one in ported one.

Since version: 20.11

### CppOverrideTestFixtureSetUp ###

**Used on**: Method
**Arguments**: None

Forces translate this method as TestFixtureSetUp, overriding the existing one (if any). Also, the forces method to be static.

### CppOverrideTestFixtureTearDowm ###

**Used on**: Method
**Arguments**: None

Forces translate this method as TestFixtureTearDown, overriding the existing one (if any). Also, the forces method to be static.

### CppPlaceAfter ###

**Used on**: Type or member
**Arguments**: Mandatory string name of type or member to place attributed one after.

Moves attributed type or member to after the one named by the attribute parameter in output code.

{{< highlight cs >}}
[CsToCppPorter.CppPlaceAfter("Base")]
public class ClassWithInlineMethods
{
    public void SayHello()
    {
        Base.SayHello();
    }
}
public class Base
{

        [CsToCppPorter.CppPlaceAfter("Property")]
        public static void SayHello()
        {
            System.Console.WriteLine("Hello");
        }
        public int Property { get; set; }
        [CsToCppPorter.CppPlaceAfter("Field")]
        public Base() { }
        private int Field;
}
{{< /highlight >}}

Ports onto:

{{< highlight cpp >}}
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
{{< /highlight >}}

**Since version**: 20.11

### CppPlaceBefore ###

**Used on**: Type or member
**Arguments**: Mandatory string name of type or member to place attributed one before.

Moves attributed type or member to before the one named by the attribute parameter in output code.

{{< highlight cs >}}
public class A
{ }
[CsToCppPorter.CppPlaceBefore("A")]
public class B
{ }
[CsToCppPorter.CppDisableAutoReordering]
public class ClassWithEnum
{
    public void Method(Enum e)
    {}
    public void Method()
    {}
    [CsToCppPorter.CppPlaceBefore("Method")]
    public enum Enum
    {
        A, B
    }
    [CsToCppPorter.CppPlaceBefore("Method")]
    public int Field;
}
{{< /highlight >}}

Ports onto:

{{< highlight cpp >}}
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
{{< /highlight >}}

**Since version**: 20.11

### CppPortAsEnum ###

**Used on**: Integer const fields
**Arguments**: None

Port numeric constant as enum (not enum class) with single value in it. Useful to define header-only constants (const fields are translated as static constants and require definition as an addition to header declaration.

{{< highlight cs >}}
class PortAsEnumTest
{
    [CsToCppPorter.CppPortAsEnum]
    const int EnumValue = 1;
    const int NonEnumValue = 1;
}
{{< /highlight >}}

{{< highlight cpp >}}
class PortAsEnumTest
{
    ...
    enum{EnumValue = 1};
    static const int32_t NonEnumValue;
};
const int32_t PortAsEnumTest::NonEnumValue = 1;
{{< /highlight >}}

### CppPortAsSingleton ###

**Used on**: Field
**Argument**: None; obsolete boolean argument may be used for compatibility reasons

Port field as singleton. Same behavior can be enabled globally by the deferred_init option.

{{< highlight cs >}}
class SingletonOwner
{
    static System.Collections.Generic.List<string> staticList = new System.Collections.Generic.List<string>{ "abc", "def", "g", "hi" };
    [CsToCppPorter.CppPortAsSingleton]
    static System.Collections.Generic.List<string> singletonList = new System.Collections.Generic.List<string>{ "abc", "def", "g", "hi" };
}
{{< /highlight >}}

{{< highlight cpp >}}
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
{{< /highlight >}}

### CppPortConstStringAsWChar ###

**Used on**: Field or class
**Arguments**: None

Port attributed string field or all string fields in attributed class as const char16_t* instead of System::String. Only possible for const or readonly fields with plain initializers. Resolves initialization race issues, speeds up startup.

### CppRenameEntity ###

**Used on**: Method or property
**Argument**: Optional string with new name. If absent, old name with '_' prefix or suffix will be used, depending on context.

Renames method or property in output code. Useful if you have name clash which can't be resolved on C++ side (e. g. two virtual methods with same signatures or C++ keyword used as an identifier). Affects both symbol and references to it.

{{< highlight cs >}}
[CsToCppPorter.CppRenameEntity("NewName")]
public String OldName()
{
    return "ClassName.OldName";
}
{{< /highlight >}}

{{< highlight cpp >}}
System::String ClassName::NewName()
{
    return u"ClassName.OldName";
}
{{< /highlight >}}

### CppSelfReference ###

**Used on**: Method
**Arguments**: None

Same as CppCTORSelfReference, but works on method. No auto-placement option at the moment exists for this attribute. Useful if method is called when no references to object exist (e. g. during construction or destruction).

### CppSkipDefinition ###

**Used on**: Class, struct, method
**Argument**: Optional boolean value - whether to generate exception-throwing stubs; defaults to 'true'.

Skips translating method or class definition, leaving declaration in place. Optionally, generates stubs that throw exception without actually doing anything. Use this attribute on methods you want to substitute native C++ implementation to.

{{< highlight cs >}}
class SkipDefinitionTest
{
    [CsToCppPorter.CppSkipDefinition]
    private static String Skip1()
    {
        return "C#";
    }
    [CsToCppPorter.CppSkipDefinition(true)]
    private static String Skip2()
    {
        return "C#";
    }
    [CsToCppPorter.CppSkipDefinition(false)]
    private static String Skip3()
    {
        return "C#";
    }
}
{{< /highlight >}}

{{< highlight cpp >}}
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
{{< /highlight >}}

### CppSkipEntity ###

**Used on**: Entity
**Argument**: Optional comment string. Always ignored.

Skips class, struct, enum, method, field, etc. The output code looks as if the entity was not present in the input at all. Unlike CppSkipDefinition, skips both declarations and definitions of the affected entities. Note that any references to skipped entity in ported code end up in compilation errors as

#### CppSkipTest ####

**Used on**: Method.
**Argument**: None.

Adds a GTEST_SKIP call at the beginning of the method. Does not apply to non-test methods.

### CppStaticMethod ###

**Used on**: Method
**Argument**: None

Forces method which is otherwise non-static to be translated as static.

### CppStaticVariable ###

**Used on**: Method
**Argument**: String name of variable inside attributed function

Forces argument variable as static. Affects all variables with same name. Only affects declarations containing single variable per declaration. Useful to cache large objects (e. g. string arrays, etc.).

{{< highlight cs >}}
[CsToCppPorter.CppStaticVariable("colors")]
private string GetFruitColor(string fruit)
{
    System.Collections.Generic.Dictionary<string, string> colors = new System.Collections.Generic.Dictionary<string, string> { { "banana", "yellow" }, { "apple", "red" }, { "carrot", "orange" } };
    return colors[fruit];
}
{{< /highlight >}}

## CppUnknownTypeParam ##

**Used on**: Generic type or method
**Argument**: String name of template parameter to treat as unknown type

Force treating type argument as unknown type: calling ObjectExt::UnknownToObject() and ObjectExt::ObjectToUnknown() whenever the conversion is required. Fixes some type conversion issues with type parameters.

## CppUseAlternativeSwitch ##

**Used on**: Method
**Arguments**: None

Forces using the do-while form of switch translation inside this method. Similar to alternative_string_switch option, but only affects a single method.

## CppUseReflection ##

**Used on**: Class
**Arguments**: None

Enables porter generating full reflection information for specific class. By default, no reflection info is generated.

## CppUsing ##

**Used on**: Class, interface
**Arguments**:

1. Mandatory string parameter: method name
1. Optional CsToCppPorter.CppUsingAttribute.Visibility enum parameter - closure level (defaults to public)

Generates 'using' directives that import symbols from base classes.

{{< highlight cs >}}
class UsingTestParent
{
    protected void Foo(bool b)
    { }
}
[CsToCppPorter.CppUsing("UsingTestBase::Foo")]
class UsingTestChild : UsingTestParent
{}
{{< /highlight >}}

{{< highlight cpp >}}
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
{{< /highlight >}}





## CppValueTypeParam ##

**Used on**: Generic type or method
**Argument**: String name of template parameter to treat as value type

Force treating type argument as value type: boxing, etc.

## CppVirtualInheritance ##

**Used on**: Class, interface
**Argument**: Name of the base class or interface to inherit from virtually (mandatory as there can be multiple base classes)

Forces virtual inheritance instead of direct.

## CppWeakPtr ##

**Used on**: Field
**Argument**:

1. Without parameters affects variable itself.
1. Single integer parameter denotes index of generic argument which should be passed as weak pointer rather than shared pointer.

Force holding value as weak pointer rather than shared pointer.

{{< highlight cs >}}
class WeakPtrTest_Cs
{
    public Object s;
    [CsToCppPorter.CppWeakPtr]
    public Object w;

    public List<Object> ss;
    [CsToCppPorter.CppWeakPtr]
    public List<Object> ws;
    [CsToCppPorter.CppWeakPtr(0)]
    public List<Object> sw;
    [CsToCppPorter.CppWeakPtr(0), CsToCppPorter.CppWeakPtr]
    public List<Object> ww;

    public class TwoArgs<A,B>
    {
        public A a;
        public B b;
    }

    public TwoArgs<Object,Object> sss;
    [CsToCppPorter.CppWeakPtr]
    public TwoArgs<Object,Object> wss;
    [CsToCppPorter.CppWeakPtr(0)]
    public TwoArgs<Object,Object> sws;
    [CsToCppPorter.CppWeakPtr(1)]
    public TwoArgs<Object,Object> ssw;
    [CsToCppPorter.CppWeakPtr(0), CsToCppPorter.CppWeakPtr(1)]
    public TwoArgs<Object,Object> sww;
    [CsToCppPorter.CppWeakPtr, CsToCppPorter.CppWeakPtr(0)]
    public TwoArgs<Object,Object> wws;
    [CsToCppPorter.CppWeakPtr, CsToCppPorter.CppWeakPtr(1)]
    public TwoArgs<Object,Object> wsw;
    [CsToCppPorter.CppWeakPtr, CsToCppPorter.CppWeakPtr(0), CsToCppPorter.CppWeakPtr(1)]
    public TwoArgs<Object,Object> www;
}

{{< /highlight >}}

{{< highlight cpp >}}
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
    System::SharedPtr<System::Collections::Generic::List<System::WeakPtr<System::Object>>> sw;
    System::WeakPtr<System::Collections::Generic::List<System::WeakPtr<System::Object>>> ww;
    System::SharedPtr<WeakPtrTest_Cs::TwoArgs<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>> sss;
    System::WeakPtr<WeakPtrTest_Cs::TwoArgs<System::SharedPtr<System::Object>, System::SharedPtr<System::Object>>> wss;
    System::SharedPtr<WeakPtrTest_Cs::TwoArgs<System::WeakPtr<System::Object>, System::SharedPtr<System::Object>>> sws;
    System::SharedPtr<WeakPtrTest_Cs::TwoArgs<System::SharedPtr<System::Object>, System::WeakPtr<System::Object>>> ssw;
    System::SharedPtr<WeakPtrTest_Cs::TwoArgs<System::WeakPtr<System::Object>, System::WeakPtr<System::Object>>> sww;
    System::WeakPtr<WeakPtrTest_Cs::TwoArgs<System::WeakPtr<System::Object>, System::SharedPtr<System::Object>>> wws;
    System::WeakPtr<WeakPtrTest_Cs::TwoArgs<System::SharedPtr<System::Object>, System::WeakPtr<System::Object>>> wsw;
    System::WeakPtr<WeakPtrTest_Cs::TwoArgs<System::WeakPtr<System::Object>, System::WeakPtr<System::Object>>> www;
};
{{< /highlight >}}

## CppWrapInMacro ##

**Used on**: Method
**Argument**: Mandatory string expression to be tested as an argument of '#if' statement

Wrap method in macro.

{{< highlight cs >}}
private class FooContainer
{
    [CsToCppPorter.CppWrapInMacro("defined(FOO_FUNCTION_EXISTS)")]
    public static void Foo()
    {}
}
{{< /highlight >}}

{{< highlight cs >}}

class FooContainer
{

    if defined(FOO_FUNCTION_EXISTS)
        static void Foo();
    endif
}
{{< /highlight >}}

## .NET attributes ##

This section enlists .NET attributes recognized by porting applicaton.

##= System.Diagnostics.Conditional ###

**Used on**: Methods
**Argument**: Mandatory string name of preprocessor definition

Wraps method body with '#if defined' directives with attribute argument as definition name. Multiple attributes join using 'logical or' coupling.

{{< highlight cs >}}
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
{{< /highlight >}}

{{< highlight cpp >}}
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
{{< /highlight >}}

##= System.ThreadStatic ###

**Used on**: static fields
**Arguments**: None

Puts static variable into thread scope. In C++, translates into 'thread_local' scope specifier.

{{< highlight cs >}}
class MyThread
{
    public int value0;
    [ThreadStatic]
    public static int value1 = 0;
    [ThreadStatic]
    public int value2 = 0;
    public static int value3 = 0;
}
{{< /highlight >}}

{{< highlight cpp >}}
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
{{< /highlight >}}

### NUnit attributes ###

This section enlists supported attributes from NUnit framework. For more information on how to use these methods, please refer to NUnit manual.

The below example shows usage of some NUnit attributes supported by CodePorting.Native Cs2Cpp.

{{< highlight cs >}}
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
{{< /highlight >}}

For more information, please refer to NUnit site at [http:~~/~~/nunit.org/](http://nunit.org/||shape#"rect").

## NUnit.Framework.Category ##

**Used on**: TestFixture class methods

Maps test methods into categories allowing porting application excluding them based on category name.

## NUnit.Framework.ExpectedException ##

**Used on**: TestFixture class methods

Marks test method with expected exception information.

## NUnit.Framework.Explicit ##

**Used on**: TestFixture class methods

Marks method as explicit test.

## NUnit.Framework.Ignore ##

**Used on**: TestFixture class methods

Ignores test (by adding 'DISABLED_' prefix to C++ test name).

## NUnit.Framework.OneTimeSetUp ##

**Used on**: TestFixture class methods

Marks method as a test fixture setupper.

## NUnit.Framework.OneTimeTearDown ##

**Used on**: TestFixture class methods

Marks method as a test fixtuer teardowner.

## NUnit.Framework.SetCulture ##

**Used on**: TestFixture class methods

Forces using specific culture when running test method.

## NUnit.Framework.SetUp ##

**Used on**: TestFixture class methods

Marks method as a test setupper.

## NUnit.Framework.Sequential ##

**Used on**: TestFixture class methods

Marks test as sequential.

## NUnit.Framework.TearDown ##

**Used on**: TestFixture class methods

Marks method as a test teardowner.

## NUnit.Framework.Test ##

**Used on**: TestFixture class methods

Marks method as a test,

## NUnit.Framework.TestCase ##

**Used on**: TestFixture class methods

Adds test case based on attributed method.

## NUnit.Framework.TestCaseSource ##

**Used on**: TestFixture class methods

Specifies test case.

## NUnit.Framework.TestFixture ##

**Used on**: Classes
**Arguments**: None

Marks class as test fixture.

## NUnit.Framework.TestFixtureSetUp ##

**Used on**: TestFixture class methods

Marks method as a test fixture setupper.

## NUnit.Framework.TestFixtureTearDown ##

**Used on**: TestFixture class methods

Marks method as a test fixture teardowner.

## NUnit.Framework.Timeout ##

**Used on**: TestFixture class methods

Sets up timeout for test (e. g. for performance testing).

## NUnit.Framewor.Values ##

**Used on**: Test method arguments

Specifies values to run test with.

# xUnit framework attributes #

This section enlists supported xUnit framework attributes. For more information please refer to xUnit site at [https:~~/~~/github.com/xunit/xunit](https://github.com/xunit/xunit||shape#"rect").

## Xunit.Fact ##

**Used on**: Methods

Marks method as fact.

## Xunit.InlineData ##

**Used on**: Methods

Specifies theory inline data.

## Xunit.Theory ##

**Used on**: Methods

Marks method as theory.
