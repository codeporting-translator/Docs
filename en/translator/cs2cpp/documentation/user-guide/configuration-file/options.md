# Configuration file options #

There is a number of options that can be used with translator config. The general syntax to add an option is as follows:

```xml
<porter>
    <opt name="option_name_1" value="option_value_1"/>
    <opt name="option_name_2" value="option_value_2" attribute1="attribute1_value"/>
    <opt name="option_name_3">Some text</opt>
    ...
</porter>
```

The options can be split into several categories. This page presents full list of available options.

[TOC]

## Output files control options ##

These options control how output files are generated.

### compare_cpp_hash ###

Allows it to skip overwriting the translated cpp and h files if the hash of their contents already matches such of the translated code to be saved here. Useful if you're frequently changing and translating C# code without cleaning the output directory and want to avoid recompiling all C++ files (overwriting the same contents updates file timestamp so that the build system will mark it as changed even if the contents is same).

| Allowed value | Meaning
| --- | ---
| true | Do not overwrite files if hashes match
| false | Overwrite files unconditionally

**Default value**: true

### write_bom ###

Whether to add BOM record at the beginning of each output C++ file.

| Allowed value | Meaning
| --- | ---
| true | Add BOM
| false | Do not add BOM

**Default value**: false

### write_include_map ###

States whether to create include map file which then can be used to manage includes from the dependent project. If you plan to translate another project which depends on the current one, creating typemap is the safest way to share type information between them.

| Allowed value | Meaning
| --- | ---
| true | Create typemap
| false | Do not create typemap

**Default value**: true

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
| --- | --- | --- | --- | ---
| public_only | Whether to exclude non-public types from the map | **true** - dump public types only; **false** = dump all files | No | true
| with_dir_prefix | Whether to use full (with directory prefix) includes or project-local ones | **true** - write full includes; **false** - write local includes | No | true

### tab ###

Indent substitution. You can use '\n' and '\t' references there as well as other in-line characters.

| Allowed value | Meaning
| --- | ---
| String value | Exact string to define one level of indentation

**Default value**: Four space characters

### endl ###

Line end substitution. You can use '\n', '\r' and '\t' references there.

| Allowed value | Meaning
| --- | ---
| String value | Exact string to define line break

**Default value**: '\n' aka line break

### low_case_file_names ###

Whether output file names are all lowercase. In this mode, word borders in Camel case class names become underscores.

| Allowed value | Meaning | Example
| --- | --- | ---
| true | All output file names are in lower case. | 'MyNewClass' class resides in header file called 'my_new_class.h'
| false | All output file names keep case of original type name. | 'MyNewClass' class resides in header file called 'MyNewClass.h'

**Default value**: false

### force_public_headers ###

Enables integrity support for public header files.

| Allowed value | Meaning
| --- | ---
| true | Checks that all header files included in public header files are in the 'include /' directory. If not, then transfers them into it.
| false | Header files included in the public header files remain in the 'source /' directory.

**Default value**: false

## Type subsystem options ##

These options define how translator behaves regarding the translation of the types.

### forwarding_if_possible ###

Use forward declarations in header files instead of includes if possible. This affects function parameters and return values declarations as well as pointers. This does not impact inheritance or value type field cases.

| Allowed value | Meaning
| --- | ---
| true | Use forward declaration instead of include.
| false | Use include instead of forward declaration.

True

```cpp
namespace MyProject {
class A;
class B
{
public:
    System::SmartPtr<A> Value;
};
} // namespace MyProject
```

False

```cpp
#include "classes/A.h"
namespace MyProject {
class B
{
public:
    System::SmartPtr<A> Value;
};
} // namespace MyProject
```

**Default value**: true

### external_object_methods ###

Use static implementations of 'ToString', 'GetType', 'GetHashCode', 'Equals' methods and 'is' operator located in ObjectExt class instead of the ones provided by Object class itself. Try this option if you experience problems with these methods being called for primitive types or generic type parameters or alike.

| Allowed value | Meaning
| --- | ---
| true | Use external object methods.
| false | Use in-object methods.

True

```cpp
template <typename T>
class A
{
public:
    System::String ValueToString()
    {
        return System::ObjectExt::ToString(m_value);
    }
private:
    T m_value;
};
```

False

```cpp
template <typename T>
class A
{
public:
    System::String ValueToString()
    {
        return m_value->ToString();
    }
private:
    T m_value;
};
```

**Default value**: true

### cast_delegate ###

When being a method parameter, cast delegate function to the actual type of expected parameter. Helps to resolve ambiguity in some cases (in C++, lambdas are not of corresponding MulticastDelegate type which can result in ambiguity if more than one signature exist).

| Allowed value | Meaning
| --- | ---
| true | Add cast expression.
| false | Do not add cast expression.

True

```cpp
ApplyDelegate(static_cast<typename DelegateType>([](String s) { return s; }));
```

False

```cpp
ApplyDelegate([](String s) { return s; });
```

**Default value**: true

### exception_as_reference ###

Whether to pass exceptions to functions by reference rather than by creating a local copy of an exception object.

| Allowed value | Meaning
| --- | ---
| true | Pass exceptions as references.
| false | Pass exceptions by value.

True

```cpp
class MyClass
{
public:
    void HandleException(Exception &e);
};
```

False

```cpp
class MyClass
{
public:
    void HandleException(Exception e);
};
```

**Default value**: false

### force_dynamic_cast ###

```xml
<opt name="force_dynamic_cast" from_type="A.B.ClassA" to_type="A.B.ClassB" />
```

For two given types, forces dynamic_cast to be used instead of default static_cast. Use if auto cast type deduction fails (e. g. due to diamond problem).

| Additional attribute | Meaning | Allowed values | Mandatory
| --- | --- | --- | ---
| from_type | Source type | Full type name | Yes
| to_type | Target type | Full type name | Yes

### force_static_cast ###

Enables translator generate ForceStaticCast casts when translating any C-style cast instead of StaticCast or DynamicCast. The ForceStaticCast always works via simple static_cast, so this option can only use if you are sure that in translated code each dynamic_cast will succeed.

| Allowed value | Meaning
| --- | ---
| true | Perform all casts via dynamic_cast (through DynamicCast or StaticCast)
| false | Perform all casts via static_cast (through ForceStaticCast)

**Default value**: false

## C# code analysis options ##

These options impact how the original C# code gets analyzed

### unexpected_override_as_warning ###

Makes translator produce warning rather than en error if there is a method that overrides one in C++ but not in C#. They may trigger from e. g. the following code:

```cs
class Base
{
    public virtual void Foo() {}
}
class Child : Base
{
    public new virtual void Foo() {} // Overrides in C++, but not in C#
}
```

| Allowed value | Meaning
| --- | ---
| true | Produce warnings if unexpected overrides occur.
| false | Produce errors if unexpected overrides occur.

**Default value:** false

### use_buildalyzer ###

Makes translator use Buildalyzer library to pre-compile SDK-styled csproj files before translating.

| Allowed value | Meaning
| --- | ---
| true | Use Buildalyzer library to pre-compile the project.
| false | Use MSBuild to pre-compile the project.

**Default value:** false

## C++ code generation parameters ##

These options define how translator uses specific C++ code features.

### detect_const_methods ###

Tells the compiler to generate const methods and insert const_cast where necessary at its own discretion. Manual attribute setting always prevails.

| Allowed value | Meaning
| --- | ---
| true | Manage const methods automaticly.
| false | Manage const methods manually.

**Default value**: false

### exclude_volatile ###

Whether to pass 'volatile' flag from C# to C++.

```cs
class Foo
{
    private volatile int m_bar;
}
```

| Allowed value | Meaning
| --- | ---
| true | Mark members as volatile
| false | Do not mark members as volatile

True

```cpp
class Foo
{
    volatile int m_bar;
};
```

False

```cpp
class Foo
{
    int m_bar;
};
```

**Default value**: false

### put_enum_on_top ###

Whether enum declarations preceed class and struct ones in output header files.

```cs
class A {}
enum B { C, D };
```

| Allowed value | Meaning
| --- | ---
| true | Enums are translated first
| false | Order is unchanged

True

```cpp
enum class B { C, D };
class A { ... };
```

False

```cpp
class A { ... };
enum class B { C, D };
```

**Default value**: false

### reorder_class_by_inheritance ###

Reorder class if dependent type declared before dependee.

```cs
class C : B {}
class B : A {}
class A {}
```

| Allowed value | Meaning
| --- | ---
| true | Dependees are translated first
| false | Order is unchanged

```cpp
class A {}
class B : A {}
class C : B {}
```

```cpp
class C : B {}
class B : A {}
class A {}
```

**Default value**: false

### alternative_string_switch ###

Define whether to use if-else or do-while form of string switch translation.

```cs
string s = "abc", s2;
switch (s)
{
case "abc":
    s2 = "cba";
    break;
case "123":
    s2 = "321";
    break;
}
```

| Allowed value | Meaning
| --- | ---
| true | Use do-while form.
| false | Use if-else form.

True

```cpp
do {
    if (s == u"abc")
    {
        s2 = u"cba";
        break;
    }
    if (s == u"123")
    {
        s2 = u"321";
        break;
    }
} while (false);
```

False

```cpp
if (s == u"abc")
{
    s2 = u"cba";
}
else if (s == u"123")
{
    s2 = u"321";
}
```

**Default value**: false

### alternative_null_coalescing ###

Use an alternative form of '??' operator translation which avoids it calculating right hand operand unless it is used.

```cs
Object obj = obj1 ?? new Object();
```

| Allowed value | Meaning
| --- | ---
| true | Use alternative form.
| false | Use usual form.

True

```cpp
System::SharedPtr<System::Object> obj = System::ObjectExt::Coalesce(obj1, [&](){return System::MakeObject<System::Object>();});
```

False

```cpp
System::SharedPtr<System::Object> obj = obj1 != nullptr ? obj1 : System::MakeObject<System::Object>();
```

**Default value**: false

### remove_unused_namespaces ###

Remove unused 'using namespace' directives (the references to namespaces no classes from which ones are used). Such constructs can result in compilation errors: if the classes from the namespace are not used, it is possible that no includes introducing this namespace exist, so the name does not get recognized by the compiler.

```cs
using System;
using System.Collections.Generic;
class MyClass
{
    void Foo()
    {
        BitConverter.GetBytes(123);
    }
}
```

| Allowed value | Meaning
| --- | ---
| true | Remove unused namespaces
| false | Keep unused namespaces

True

```cpp
#include "MyClass.h"

using namespace System;

void MyClass::Foo()
{
    BitConverter::GetBytes(123);
}
```

False

```cpp
#include "MyClass.h"

using namespace System;
using namespace System::Collections::Generic;

void MyClass::Foo()
{
    BitConverter::GetBytes(123);
}
```

**Default value**: true

### indexer_as_method ###

Defines whether to translate indexer invocation as method instead of operator [] even if the later form is possible.

```cs
System.Collections.Generic.List<int> mylist = GetList();
int i = mylist[0];
```

| Allowed value | Meaning
| --- | ---
| true | Translate indexers as methods
| false | Translate indexers as operators

True

```cpp
System::Collections::Generic::ListPtr<int> mylist = GetList();
int i = mylist->idx_get(0);
```

False

```cpp
System::Collections::Generic::ListPtr<int> mylist = GetList();
int i = mylist[0];
```

**Default value**: true

### use_weak_ptr_std_bind ###

When generating std::bind() expressions for delegates translating, use WeakPtr instead of raw C++ pointers to pass object reference.

```cs
delegate string ModifyString(string str);
class WithDelegate
{
    private string prefix = "prefix_";
    public string AddPrefix(string str)
    {
        return prefix + str;
    }
    public void Foo()
    {
        ModifyString myDelegate = AddPrefix;
    }
}
```

| Allowed value | Meaning
| --- | ---
| true | Use WeakPtrs instead of raw C++ pointers.
| false | Use raw C++ pointers.

True

```cpp
ModifyString myDelegate = std::bind(&WithDelegate::AddPrefix, System::WeakPtr<WithDelegate>(this), std::placeholders::_1);
```

False

```cpp
ModifyString myDelegate = std::bind(&WithDelegate::AddPrefix, this, std::placeholders::_1);
```

**Default value**: false

### deferred_init ###

If enabled, replaces all static fields with singletons and calls static constructors from instance constructors and singleton access functions instead of C++ static objects initializers. Use this option to resolve the static objects initialization races. Slows down static fields access and constructors as there are additional checks.

| Allowed value | Meaning | Description
| --- | --- | ---
| None | Disabled | Static constructors are translated as constructors of global static variables. Static class fields are translated as static class fields.
| All | Enabled for all classes | Static constructors are translated as static functions. Static class fields are translated as singletons. Constructors and singleton accessors call into static constructor to make sure it is finished before object creation or static variable access.
| Tests | Enabled for test classes only | Static constructors of TestFixture classes are translated as static functions. TestFixture classes static fields are translated as singletons. Constructors and singleton accessors of TestFixture classes call into static constructor to make sure it is finished before object creation or static variable access.

Static constructors of non-TestFixture classes are translated as constructors of global static variables. Static fields of non-TestFixture classes are translated as static class fields.

**Default value**: None

### auto_ctor_self_reference ###

If enabled, puts constructor self reference guards where required, allowing it for constructor to refer to 'this' without deleting the object. Saves the developer from putting CppCtroSelfReference attributes manually but creates more guards than actually required.

```cs
class MyClass
{
    public MyClass()
    {
        SomeOtherClass.DoSomething(this);
    }
}
```

| Allowed value | Meaning
| --- | ---
| true | Place guards that allow safe usage pf shared pointers to object being constructed
| false | Do not place guards automatically. Make sure to use [CppCtorSelfReference](../cpp-attributes/reference.md#cppctorselfreference) attributes manually, otherwise you will have a 'deletion in constructor' issue.

True

```cpp
MyClass::MyClass()
{
    IncSelfReference();
    auto __local_self_ref = System::MakeScopeGuard([this]{ DecSelfReference(); });
    SomeOtherClass::DoSomething(System::MakeSharedPtr(this));
}
```

False

```cpp
MyClass::MyClass()
{
    SomeOtherClass::DoSomething(System::MakeSharedPtr(this));
}
```

**Default value**: true

### force_add_shared_api_macros ###

If enabled, forces production of shared_api_defs.h file and inserts corresponding macros into the translated code. This helps to switch between shared and static library project using the make_shared_lib option but without re-translating whole project.

| Allowed value | Meaning
| --- | ---
| true | Create shared_api_defs.h file regardless which library type (shared or dynamic) is targetted
| false | Create shared_api_defs.h file only if targetting shared library.

**Default value**: false

### finally_statement_as_lambda ###

Allows translating the try-finally statement as a lambda expression instead of guard object placement.

```cs
try
{
    InnerMethod();
}
finally
{
    Console.WriteLine("Finally");
    throw new Exception();
}
```

| Allowed value | Meaning
| --- | ---
| true | try-finally statement is translated through lambdas.
| false | try-finally statement is translated using sentry object.

True

```cpp
System::DoTryFinally([&] /* try-catch block */
{
    InnerMethod();
}
, [&] /* finally block */
{
    System::Console::WriteLine(u"Finally");
});
```

False

```cpp
auto __finally_guard_0 = ::System::MakeScopeGuard([]()
{
    System::Console::WriteLine(u"Finally");
    throw System::Exception();
});

try
{
    InnerMethod();
}
catch (...)
{
    throw;
}
```

**Default value**: false

### setter_wrap_with_lambda ###

Forces translating complex property assignment operators using lambdas.

```cs
obj.PublicProperty += "abc";
```

| Allowed value | Meaning
| --- | ---
| true | Complex property assignments use lambdas.
| false | Complex property assignments are translated using default approach.

True

```cpp
System::WithLambda::setter_add_wrap(GETTER_SETTER_LAMBDA_ARGS(obj, PublicProperty), u"abc")
```

False

```cpp
System::setter_add_wrap(static_cast<ConcreteBase*>(obj.GetPointer()), &ConcreteBase::get_PublicProperty, &ConcreteBase::set_PublicProperty, u"abc");
```

**Default value**: false

### allow_interface_members_base_class_impl ###

In C++, members of interface can be implemented in the base class. In C#, there's no way doing so. This option generates required calls in child class; however, this can overcomplicate output code in some cases.

```cs
public interface IFoo
{
   void Do(int i, string s);
}
public class FooImpl
{
    public void Do(int i, string s)
    {
    }
}
public class Foo : FooImpl, IFoo
{
}
```

| Allowed value | Meaning
| --- | ---
| true | Adds required calls to methods implemented in base classes.
| false | Doesn't generate required calls.

True

```cpp
class Foo : public FooImpl, public IFoo
{
    void Do(int32_t i, System::String s);
};
void Foo::Do(int32_t i, System::String s)
{
    FooImpl::Do(i, s);
}
```

**Default value**: true

### polymorphic_memberwiseclone ###

```xml
<opt name="polymorphic_memberwiseclone" value="true">
    <root class="MyNamespace.MyClass"/>
    <root class="MyNamespace.MyClass2"/>
</opt>
```

By default, MemberwiseClone() method in translated code slices output object to the type it is called for. All information about child classes is lost for all implementation is static. This option allows injecting additional virtual methods to the classes MemberwiseClone() is called for and to their child classes. This fixes MemberwiseClone() behavior, but generates additional code. Please note that translator only considers MemberwiseClone() calls located in same assembly by default and doesn't generate additional code for classes which are not subjects for MemberwiseClone() calls. To force generating these methods for specific classes and their subclasses (e. g. if MemberwiseClone() is called from different assembly), use 'root' subnodes with mandatory 'class' attributes containing C# class names.

| Allowed value | Meaning
| --- | ---
| true | MemberwiseClone() clones full class tree.
| false | MemberwiseClone() cuts class tree being copied up to the class it is called upon.

**Default value**: false

### version_compatibility_check_mode ###

Allows translator generate code which compares headers version used to compile project and supplied library version on startup.

| Allowed value | Meaning
| --- | ---
| stderr | On version mismatch, write error message to stderr, add a record to modules version mismatch registry and continue
| stdout | On version mismatch, write error message to stdout, add record to modules version mismatch registry and continue
| silent | No output; on version mismatch, add a record to modules version mismatch registry and continue
| exit | On version mismatch, call std::exit(EXIT_FAILURE)
| none | Don't add version check code to the resulting project

**Default value**: stderr

### force_const_auto_property_getter ###

Marks auto-generated property getters as const methods.

| Allowed value | Meaning
| --- | ---
| true | Mark auto-generated property getters as const.
| false | Do not mark auto-generated property getters const.

**Default value**: false

### force_const_simple_property_getter ###

Marks simple property getters consisting of single 'return field_name' statement as const.

| Allowed value | Meaning
| --- | ---
| true | Mark property getters like 'return field_name' as const.
| false | Keep such property getters non-const.

**Default value**: false

### process_base_overloading ###

Puts 'using' statement to re-declare hidden baseclass methods in subclasses.

| Allowed value | Meaning
| --- | ---
| true | Add using statements
| false | Do not add using statements

True

```cpp
class Bar {
    ...
    void Do();
};
class Foo : public Bar {
    ...
    void Do(System::String s);
    using Bar::Do;
};
```

False

```cpp
class Bar {
    ...
    void Do();
};
class Foo : public Bar {
    ...
    void Do(System::String s);
};
```

**Default value**: false

### thread_static_generation ###

Determines how to translate ThreadStatic attribute.

| Allowed value | Meaning
| --- | ---
| disabled | Ignore ThreadStatic attribute
| native | Translate ThreadStatic attribute as thread_local storage class.
| singleton | Convert such fields into singletons.

Disabled

```cpp
static System::String m_value;
```

Native

```cpp
static thread_local System::String m_value;
```

Singleton

```cpp
static System::String m_value() { static thread_local value = false; return value; }
```

**Default value**: native

### remove_inactive_code ###

Drops comments with inactive code.

| Allowed value | Meaning
| --- | ---
| false | Keep comments with inactive code.
| true | Drop inactive code silently.

**Default value**: false

### emit_preprocessor_directives ###

Propagates preprocessor directives to C++.

| Allowed value | Meaning
| --- | ---
| false | Drop preprocessor directives.
| true | Add comments on used preprocessor directives.

**Default value**: true

### emplace_assembly_details ###

Replaces calls into Assembly::Get*Assembly() with calls to project-local GetAssembly_ProjectName(). Unbinds resources from global variables, hides them into local singleton instead.

| Allowed value | Meaning
| --- | ---
| false | Use global singletons for Assembly.
| true | Use project-local singletons for Assembly.

**Default value**: false

### nunit_assert_class_aliases ###

Calls to methods of NUnit.Framework.Assert class are translated into gtest-compatible macros. If you use your own test class with similarly named methods, use this option to enable special treatment of these. Enlist classes wrapped inside 'alias' subnode as shown below. Also, you might want to exclude such classes from translating.

```xml
<opt name="nunit_assert_class_aliases" value="true">
    <alias class="MyNamespace.MyAssertClass"/>
</opt>
```

| Allowed value | Meaning
| --- | ---
| true | Allow Assert-like classes special treatment.
| false | Translate Assert-like classes as usual.

**Default value**: false

### original_tests_names ###

Toggles prefixing test name with category name to simplify tests group run after translating. Alternatively, if you prefer having original tests names, you might want to disable this option.

```xml
<opt name="original_tests_names" value="true"/>
```

```cs
[TestFixture]
public class OriginalTestName
{
    [Test]
    [Category("Original")]
    public void Test1() {}
}
```

| Allowed value | Meaning
| --- | ---
| true | Do not add category prefix.
| false | Add category prefix.

True

```cpp
TEST_F(OriginalTestName, Test1) { s_instance->Test1(); }
```

False

```cpp
TEST_F(OriginalTestName, Original_Test1) { s_instance->Test1(); }
```

**Default value**: false

### cpp_enum_enable_metadata ###

Enables metadata globally, same as [CppEnumEnableMetadata](../cpp-attributes/reference.md#cppenumenablemetadata) does for individual enums.

```xml
<opt name="cpp_enum_enable_metadata" value="true"/>
```

| Allowed value | Meaning | Description
| --- | --- | ---
| true | Generate metadata for all public enums. | Enum to string and string to enum conversions provide full text information, same as in C#.
| false | Generate metadata for enums marked with CppEnumEnableMetadata attribute only. | Unmarked enums convert to string and from string in numeric format only.

**Default value**: false

### hide_forward_declarations ###

Wraps forward declarations section into '@cond...@endcond' section to forbid Doxygen process it.

| Allowed value | Meaning
| --- | ---
| true | Wrap forward declaration into Doxygen conditional block.
| false | Do not wrap forward declaration into Doxygen conditional block.

True

```cpp
/// @cond
namespace SomeNS { class Class1; }
/// @endcond
```

False

```cpp
namespace SomeNS { class Class1; }
```

**Default value:** false

### attributes_into_reflection_info ###

Makes translator propagate information on specific attributes into reflection tables. Use like the following:

```xml
<opt name="attributes_into_reflection_info">
    <attribute>JsonIgnore</attribute>
</opt>
```

'Attribute' subnode with attribute name text specifies the attribute to propagate.

**Default value:** attributes do not get propagated into reflection information

### allow_cast_to_non_generic_list ###

Allows casts to System.Collections.IList to be translated into compilable code.

| Allowed value | Meaning
| --- | ---
| true | Such casts work in translated code.
| false | Such casts do not compile. This is a recommended behavior, as .Net 1.0 collection support is legacy in most C# projects.

**Default value:** false

### fix_setter_return_tag ###

Replaces \<retruns\> tag with \<param name="value"\> for property setters.

```cs
class A
{
    /// <summary>
    /// Foo getter and setter
    /// </summary>
    /// <returns>foo</returns>
    public int Foo
    {
        get { return foo; }
        internal set { foo = value; }
    }

    int foo;
}
```

| Allowed value | Meaning
| --- | ---
| true | Replaces \<returns\> tag for setter with \<param\> tag
| false | Leave all as is

True

```cpp
class A : public System::Object
{
public:
    /// <summary>
    /// Foo getter and setter
    /// </summary>
    /// <returns>foo</returns>
    int32_t get_Foo();
    /// <summary>
    /// Foo getter and setter
    /// </summary>
    /// <param name="value">foo</param>
    void set_Foo(int32_t value);
 };
```

False

```cpp
class A : public System::Object
{
public:
    /// <summary>
    /// Foo getter and setter
    /// </summary>
    /// <returns>foo</returns>
    int32_t get_Foo();
    /// <summary>
    /// Foo getter and setter
    /// </summary>
    /// <returns>foo</returns>
    void set_Foo(int32_t value);
};
```

**Default value:** false

### remove_all_comments ###

Removes all comments from sources.

**Default value:** false

### explicit_destructors ###

Generates destructors for each translated class or struct.

```cs
struct A {}
class B {}
```

| Allowed value | Meaning
| --- | ---
| true | Adds generated desctructor
| false | Destructor is not generated

True

```cpp
class A : public System::Object
{
public:
    ~A() {}
}

class B : public System::Object
{
public:
    ~B() {}
}
```

False

```cpp
class A : public System::Object
{
}

class B : public System::Object
{
}
```

**Default value**: false

### force_wrap_iostream ###

Overloads all methods that accept System::IO::Stream arguments, as if [CppIOStreamWrapper](../cpp-attributes/reference.md#cppiostreamwrapper) was present.

```cs
public void IStream(Stream istream)
{
    ...
}
```

| Allowed value | Meaning
| --- | ---
| true | Generate overload.
| false | Do not generate overload.

True

```cpp
void IStream(System::SharedPtr<System::IO::Stream> istream);
template <typename CharType, typename Traits = std::char_traits<CharType>>
void IStream(std::basic_istream<CharType, Traits>& istream)
{
    auto istreamWrapper = System::IO::WrapSTDIOStream(istream);
    IStream(istreamWrapper);
}
```

False

```cpp
void IStream(System::SharedPtr<System::IO::Stream> istream);
```

**Default value**: false

### allow_using_directives_in_headers ###

Makes translator simplify header files by utilizing header directives.

```cs
using Namespace1;
using System;
namespace Namespace2
{
    public class Class2
    {
        public void Foo(Class1 c1) { ... }
    }
}
```

| Allowed value | Meaning
| --- | ---
| true | Simplify the code.
| false | Use full type qualifiers.

True

```cpp
using namespace Namespace1;
using namespace System;
namespace Namespace2
{
    class Class2
    {
    public:
        void Foo(SharedPtr<Class1> c1);
    };
}
```

False

```cpp
namespace Namespace2
{
    class Class2
    {
    public:
        void Foo(System::SharedPtr<Namespace1::Class1> c1);
    };
}
```

**Default value**: false

### extensions_as_method ###

```xml
<opt name="extensions_as_method" value="true">
    <extension class="Aspose.BarClassExtensions"/>
</opt>
```

Specifies the classes for which extension method calls should be translated as member function calls instead of a static method from extension class. Value is ignored.

```cs
obj.CallExtensionMethod(arg);
```

Extension type is meitioned in 'extension' node under 'opt' config node. Generate method call instead of static function call.

```cpp
obj->CallExtensionMethod(arg);
```

Extension type is not meitioned in 'extension' node under 'opt' config node. Generate static function call rather than method call.

```cpp
ExtensionClass::CallExtensionMethod(obj, arg);
```

**Default value**: true

### use_literals ###

Allows translator to use C++ literal to simplify some casts.

```cs
foo("Hello");
```

| Allowed value | Meaning
| --- | ---
| true | Use literals.
| false | Use constructor-like casts.

True

```cpp
foo(u"Hello"_s);
```

False

```cpp
foo(System::String(u"Hello"));
```

**Default value**: false

### simplified_initializers ###

Allows translator to use trailing expressions object creation syntax.

| Allowed value | Meaning
| --- | ---
| true | Use trailing expression syntax.
| false | Use lambda syntax.

**Default value**: true

### inline_numeric_constants ###

Allows translator to inline numeric constants definitions in header files.

| Allowed value | Meaning
| --- | ---
| true | Inline numeric constants as compile-time expressions.
| false | Keep nummeric constants as runtime objects.

**Default value**: true

### generate_begin_end_methods ###

Allows the translator to generate begin(), end() and other STL-like iterators access methods for those classes implementing the generic IEnumerable interface.

If the class impelements the generic IEnumerable interface via returning GetEnumerator() call of its field or auto-property, and the type of this field or auto-property provides begin() and end() methods, these methods will be proxied at the class level. For example, the following code will go:

```cs
public class Class0 : IEnumerable<int>
{
    ...
    protected List<int> list; // List has begin/end methods.
    public IEnumerator<int> GetEnumerator()
    {
        return list.GetEnumerator(); // doing nothing but return list.GetEnumerator()
    }
    ...
}
```

If the implementation of GetEnumerator() is more complex, or the type of the property or field it operates doesn't provide begin() and end() methods, these won't be generated at class level, too. The following classes are not eligable for begin() and end() methods generation:

```cs
public class Class1 : IEnumerable<int>
{
    ...
    protected List<int> list; // List has begin/end methods.
    public IEnumerator<int> GetEnumerator()
    {
        list = new List<int>() { 1, 2, 3 }; // Modifying member before calling GetEnumerator()
        return list.GetEnumerator();
    }
    ...
}
public class Class2 : IEnumerable<int>
{
    ...
    protected Class1 list; // Class1 has no begin/end methods.
    public IEnumerator<int> GetEnumerator()
    {
        return list.GetEnumerator(); // doing nothing else than return list.GetEnumerator()
    }
    ...
}
```

This behavior can be overwritten by using [CppNoBeginEndMethods](../cpp-attributes/reference.md#cppnobeginendmethods) or [CppGenerateBeginEndMethods](../cpp-attributes/reference.md#cppgeneratebeginendmethods) attributes regardless of the option's value.

| Allowed value | Meaning
| --- | ---
| true | Generate iterator methods.
| false | Do not generate iterator methods.

True

```cpp
class Class0 : public System::Collections::Generic::IEnumerable<int32_t>
{
    ...
public:
    /// A collection type whose iterator types is used as iterator types in the current collection.
    using iterator_holder_type = System::Collections::Generic::List<int32_t>;
    /// Iterator type.
    using iterator = typename iterator_holder_type::iterator;
    /// Const iterator type.
    using const_iterator = typename iterator_holder_type::const_iterator;
    System::SharedPtr<System::Collections::Generic::IEnumerator<int32_t>> GetEnumerator() override;
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

False

```cpp
class Class0 : public System::Collections::Generic::IEnumerable<int32_t>
{
    ...
public:
    System::SharedPtr<System::Collections::Generic::IEnumerator<int32_t>> GetEnumerator() override;
    ...
};
```

**Default value:** true

### default_lambda_capture_mechanism ###

Specifies the lambda capturing mechanism.

| Allowed value | Meaning
| --- | ---
| pass_by_reference | All lambda expressions will capture variables, parameters, etc. by reference.
| pass_by_value | All lambda expressions will capture variables, parameters, etc. by value.
| use_holders | All lambda expressions will capture variables, parameters, etc. wrapped into the `LambdaCaptureHolder` class instances.
| pass_by_reference_when_holder_is_redundant | The translator will analyze if `LambdaCaptureHolder` must be used for wrapping. Variables and `this` will be passed to lambda expressions by reference when it is possible.

Pass_by_reference

```cpp
void foo() {
  int32_t value = 10;
  LambdaCaptureTest::VoidVoidDelegate lambda = LambdaCaptureTest::VoidVoidDelegate(static_cast<std::function<void()>>([&value]() -> void {
    ASSERT_EQ(10, value);
  }));
}
```

Pass_by_value

```cpp
void foo() {
  int32_t value = 10;
  LambdaCaptureTest::VoidVoidDelegate lambda = LambdaCaptureTest::VoidVoidDelegate(static_cast<std::function<void()>>([value]() -> void {
    ASSERT_EQ(10, value);
  })).template AddHeldVariable<LambdaCaptureTest::VoidVoidDelegate>("value", value);
}
```

Use holders

```cpp
void foo() {
  System::Details::LambdaCaptureHolder<int32_t> _lch_value = 10;
  int32_t &value = _lch_value.GetCapture();
  LambdaCaptureTest::VoidVoidDelegate lambda = LambdaCaptureTest::VoidVoidDelegate(static_cast<std::function<void()>>([_lch_value, &value]() -> void {
    ASSERT_EQ(10, value);
  })).template AddHeldVariable<LambdaCaptureTest::VoidVoidDelegate>("value", value);
}
```

Pass_by_reference_when_holder_is_redundant

```txt
The translator will analyze if `LambdaCaptureHolder` must be used for wrapping. Variables and `this` will be passed to lambda expressions by reference when it is possible.
```

**Default value:** pass_by_reference_when_holder_is_redundant

### always_include_delegates ###

| Allowed value | Meaning
| --- | ---
| true | Include delegates' original declarations.
| false | Re-declare delegates in the files they are used in.

**Default value:** false

### force_const_ref_parameters ###

| Allowed value | Meaning
| --- | ---
| true | The non-virtual methods/constructors/setters/operators parameters with `String` or `SmartPtr` types are passed by const reference in a translated code.
| false | The non-virtual methods/constructors/setters/operators parameters with `String` or `SmartPtr` types are passed by value in a translated code.

**Default value:** false

### force_const_ref_return_type_for_auto_properties ###

| Allowed value | Meaning
| --- | ---
| true | Auto-property getters of shared pointer types return const referencs to backed fields.
| false | Auto-property getters of shared pointer types return copies of values of backed fields.

**Default value:** false

### force_const_ref_return_type_simple_properties ###

| Allowed value | Meaning
| --- | ---
| true | Simple property getters (that contain only one return statement) of shared pointer types return const references to backed fields.
| false | Simple property getters (that contain only one return statement) of shared pointer types return copies of values to backed fields.

**Default value:** false

### generate_struct_default_methods ###

Forces translator to generate code with default ValueType methods (operator==, Equals, ToString and GetHashCode) for all structures if these methods not defined by the user.

The option is equivalent to set [CppAddStructDefaultMethods](../cpp-attributes/reference.md#cppaddstructdefaultmethods) attribute to all structures in the project.

| Allowed value | Meaning
| --- | ---
| true | Generate default methods for all structs.
| false | Generate methods for structs only marked with [CppAddStructDefaultMethods](../cpp-attributes/reference.md#cppaddstructdefaultmethods) attribute.

**Default value:** false

### force_enum_flags_attribute ###

| Allowed value | Meaning
| --- | ---
| true | Generate operators for all enums, as if 'Flags' attribute was present.
| false | Only generate enum operators where 'Flags' attribute is present.

**Default value:** false

### alternative_debug_class ###

Makes the translator convert the calls to the methods of the specified class into macros.

C# code:

```cs
Aspose.Debug.Assert(true);
```

Configuration file:

```xml
<typemap>
    <class csname="Aspose.Debug" cppname="Aspose::Replace::NullDebug"/>
</typemap>
<opt name="alternative_debug_class" value="Aspose.Debug"/>
```

C++ code:

```cpp
NULLDEBUG_ASSERT(true);
```

**Default value:** false

### class_ptr_alias ###

Makes the translator generate a special 'Ptr' member type to each public class that aliases a smart poitner to this class.

C# code:

```cs
public class MyClass
{}
```

| Allowed value | Meaning
| --- | ---
| true | Generate 'Ptr' member type for all public classes.
| false | Do not generate 'Ptr' member type.

True

```cpp
class MyClass : public System::Object
{
    // ...
public:
    /// An alias for shared pointer to an instance of this class.
    using Ptr = System::SharedPtr<MyClass>;
}
```

False

```cpp
class MyClass : public System::Object
{
    // ...
}
```

**Default value:** false

### ignore_constraints ###

```xml
<opt name="ignore_constraints" value="true"/>
```

Skip generation of asserts that constrain types in C++ template translated from C# generic. Works same as attribute [CppIgnoreConstraints](../cpp-attributes/reference.md#cppignoreconstraints) but applied to all classes in project.

```cs
namespace IgnoreConstraintsTestOpt
{
    public class MyClass<T> where T : IEnumerable
    {}
}
```

| Allowed value | Meaning
| --- | ---
| true | Skip generation of asserts in C++ translated template.
| false | Don't skip generation of asserts in C++ translated template.

True

```cpp
template<typename T>
class MyClass : public System::Object
{
    typedef MyClass<T> ThisType;
    typedef System::Object BaseType;
   
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
   
    template<typename FT0> friend class IgnoreConstraintsOptTest::MyClass;
   
public:

    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
               
        }
    }
   
};
```

False

```cpp
template<typename T>
class MyClass : public System::Object
{
    typedef System::Collections::IEnumerable BaseT_IEnumerable;
    assert_is_base_of(BaseT_IEnumerable, T);
   
    typedef MyClass<T> ThisType;
    typedef System::Object BaseType;
   
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
   
    template<typename FT0> friend class IgnoreConstraintsOptTest::MyClass;
   
public:

    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
               
        }
    }
   
};
```

**Default value**: false

## Debug and developer version code options ##

These options control debug and developer version code in generated C++ files.

### collect_test_methods ###

Whether to collect information on translated code test methods in C++ runtime by calling "System::TestToolsExt::RegisterTest()" for each test method on initialization stage.

| Allowed value | Meaning
| --- | ---
| true | Collect information on tests
| false | Don't collect information on tests

**Default value**: false

### generate_for_each_member ###

Enables adding for_each_member subsystem-related code to each class and generating gv (graphviz) dumps of in-memory objects after each test.

| Allowed value | Meaning
| --- | ---
| true | Generate for_each_member-related code
| false | Don't generate for_each_member-related code

**Default value**: false

### for_each_member_cycles_only ###

Enables cycles search using for_each_member

| Allowed value | Meaning
| --- | ---
| true | Generate parameter passing that enables loop search
| false | Don't generate parameter passing that enables loop search

**Default value**: false

### for_each_member_cleanup_before_each_test ###

Enables cleaning up the for_each_member model before running each test.

| Allowed value | Meaning
| --- | ---
| true | Generate a method call that clears the for_each_member model inside the SetUp method
| false | Don't generate a method call that clears the for_each_member model inside the SetUp method

**Default value**: false

### stable_gv_file ###

Enables renumbering objects in for_each_member-based gv dumps so that the indexes in output files do not depend on any temporary objects being created and then destroyed during the execution. Required if you want to compare the gv file against a template and want it to remain stable in all builds (release and debug builds and Visual Studio and non-Visual Studio builds currently create different amounts of temporary objects, therefore, affecting object indexes in non-stabilized builds).

| Allowed value | Meaning
| --- | ---
| true | Renumbers objects when dumping gv files to stabilize them.
| false | Disables objects renumbering.

**Default value**: false

### test_run_stub_file ###

Creates a file to dump all tests names into during translating

| Allowed value | Meaning
| --- | ---
| \<Path to stub file\> | Path to the file to enlist all tests.
| \<Empty\> | Disables tests enlisting.

**Default value**: \<Empty\>

### insert_leakage_detectors ###

Insert helper code to detect the constructors leaking in references. This usually means that the nested objects created by this constructor refer to the object itself using shared pointers instead of weak ones which promises some big problems (memory leaks, double deletion issues on constructor exceptions, etc.). If enabling this feature, check output in debug to track potential problems.

| Allowed value | Meaning
| --- | ---
| true | Insert leakage detection code.
| false | Doesn't generage code to do the checks.

Example output message:

```txt
Shared pointer leakage: constructor MyClass::MyClass(int, int) leaked 7 references.
```

**Default value**: false

### tests_garbage_collection ###

Calls DBG_GARBAGE_COLLECTION mechanism after each test.

| Allowed value | Meaning
| --- | ---
| none | Doesn't collect garbage after tests
| report | Collects garbage and reports collected objects after each test. Class names, memory addresses, member names and addresses of objects these members point to are listed.

```txt
Island of isolation is found.
Objects:
    0x11223344: MyClass: 2 reference
        m_a: 0x55667788
        m_b: 0x99aabbcc
    0x55667799: MyClass2: 1 reference
        m_owner: 0x11223344
    0x99aabbcc: MyClass2: 1 reference
        m_owner: 0x11223344
```

**Default value**: none

### tests_garbage_collection_generation ###

GC generation to collect by __DBG_GARBAGE_COLLECTION wrappers after tests. For optimization purposes.

| Allowed value | Meaning
| --- | ---
| Integer value 0 to 2 | Same as generation used by GC in C#

**Default value**: 0

### add_category_name_to_timeout_tests ###

If defined, value is used as a category name for all tests with timeouts.

**Default value**: \<Not defined\>

### for_each_member_short_names ###

Enables short names being generated for members available through for_each_member-related functions.

| Allowed value | Meaning | Example
| --- | --- | ---
| false | Generate long names | "ForEachMemberTest::ForEachMemberTest::Child"
| true | Generate short names | "Child"

**Default value**: false

### enable_warnings_for_virtual_function_calls ###

Enables translator raising warnings if any virtual methods are called from constructor, as the behavior will be different in C++.

| Allowed value | Meaning
| --- | ---
| false | Do not generate warnings
| true | Generate warnings. Virtual function call is found in constructor/destructor definition.

**Default value**: true

## Path resolving behavior ##

This option regulate how translator check path that located in the config file.

### use_porter_home_directory_while_resolving_path ###

Enables translator using translator home directory and translator executable location when resolving paths mentioned in configuration file. Only affects declarations that go after it in configuration file. Can't be disabled if it is enabled somewhere else in configuration file.

| Allowed value | Meaning
| --- | ---
| false | Only directory with current configuration file is used as a lookup for mentioned paths.
| true | Adds translator binary location and %PorterHome% variable set either explicitly or via translator command line to the list of lookup directories.

**Default value**: false

## Project settings ##

These options specify the settings of output project.

### cmake_targets ###

Whether to build translated project, tests or both.

| Allowed value | Meaning
| --- | ---
| PortedProject | Only build translated project (application if source project is application, library if source project is library)
| Tests | Only build test application but not the translated project
| Both | Build both tests and translated project

**Default value**: Both

### make_shared_lib ###

Whether to generate shared library project or static library project. Only makes effect if building translated project is allowed (see cmake_targets option) and source project ls a library rather than executable.

| Allowed value | Meaning
| --- | ---
| false | Generate static library
| true | Generate shared library

**Default value**: false

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
| --- | --- | --- | --- | ---
| export_per_member | Whether to generate per-member export attributes instead of per-class ones. | true, false | No | true
| export_internals | Whether to add *SHARED_API macro to internal class members, not only to public class members. | true, false | No | false
| shared_id | Overrides default (generated) \*SHARED_API macro. | Prefix to \*_SHARED macro. | No | Assembly name with dots replaced with underscores

### cpp_lib_path ###

Path to system library folder (the one containing 'include/' and 'lib/' directories).

| Allowed value | Meaning
| --- | ---
| Path to library directory | Relative (to config) or absolute path

**Default value**: ../../../../asposecpplib

### cmake_temaplates or makefile_templates ###

Path to the directory with CMakeLists.txt file to be used as a template.

| Allowed value | Meaning
| --- | ---
| Path to the directory | Path or name of directory containing CMakeLists.txt template

**Default value**: cmake

### generatedlist_template ###

Path to a file which will be used as a template for outputting list of all generated sources (header and cpp files)

| Allowed value | Meaning
| --- | ---
| Path to the file | Path or name of a template file

**Default value**: "" (empty string)

Let's suppose you have the following `GeneratedList.cmake file:`

```cmake
set(generatedhpp
%%HEADERS%%
)

set(generatedcpp
%%SOURCES%%
)
```

The translator will create file `GeneratedList.cmake` next to `CMakeLists.txt` with the following content:

```cmake
set(generatedhpp
include/public_header1.h
include/public_header2.h
source/private_header3.h
source/private_header4.h
# etc.
)

set(generatedcpp
source/public_source1.cpp
source/public_source2.cpp
source/private_source3.cpp
source/prviate_source4.cpp
# etc.
)
```

Now you can include this file from `CMakeLists.txt` and use `generatedcpp` and `generatedhpp` variables instead of `file(GLOB)` cmake command.

### include_templates ###

Path to the directory with shared_api_defs.h template file used to generate shared library API.

| Allowed value | Meaning
| --- | ---
| Path to the directory | Path to the directory containing shared_api_defs.h template

**Default value**: include

### source_templates ###

Path to the directory with embedded_resources.cpp template to support Assembly class and C# project's resources access.

| Allowed value | Meaning
| --- | ---
| Path to the directory | Path to the directory containing embedded_resources.cpp template

**Default value**: source

### add_assembly_details ###

Indicates for which returned assembly current project will be used. I.e for executing_assembly Assembly.GetExecutingAssembly() will be used to access assembly name resources etc.

| Allowed value | Meaning
| --- | ---
| executing_assembly | Use assembly executing at given moment
| entry_assembly | Use assembly used as an entry point
| calling_assembly | Use assembly calling into current one

**Default value**: `None`

### additional_defines ###

Additional defines for either C# code (used during code parsing) or C++ project (passed to cmake).

| Allowed value | Meaning
| --- | ---
| List of defines to use | List of the defiles. Separators are space (' ') and semicolon (';')

**Default value**: `Empty`

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
| --- | --- | --- | --- | ---
| cmakeonly | Whether the define goes only to C++ project and not to C# project | true, false | No | false
| csonly | whether the define goes to only to C# project and not to C++ project | true, false | No | false

### exclude_conditional_symbols ###

Exclude defines from being passed from C# project to cmake. Normally, translating applications passes all definitions mentioned in project file to cmake.

| Allowed value | Meaning
| --- | ---
| List of defines | Defines in C# project that won't be passed to C++ project. Separators are space (' ') and semicolon (';')

**Default value**: `None`

### additional_includes ###

Additional includes to pass to translated project via cmake.

| Allowed value | Meaning
| --- | ---
| List of directories | List of additional include directories passed to cmake. Separators are space (' ') and semicolon (';')

**Default value**: `Not defined`

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
| --- | --- | --- | ---
| local | If this path is mentioned in translator-generated include, whether to cut generated include to relative path | true

false | No | false

### custom_gtest_main ###

Path to the custom file with gtest main() function to use instead of the default one.

| Allowed value | Meaning
| --- | ---
| Path to the file | Path to the file containing main() function to call into gtest.

**Default value**: gtest_main.cc

### cpp_files_to ###

Directory to copy cpp files contained in current project to.

| Allowed value | Meaning
| --- | ---
| Directory name | Directory name inside output project folder

**Default value**: source

### interface_as_public ###

Unconditionally move all interfaces to public headers, including non-public ones.

| Allowed value | Meaning
| --- | ---
| true | Move all interfaces to public headers
| false | Put public interfaces to public headers, put private interfaces to private headers

**Default value**: false

### internal_as_public ###

Whether to translate internal members and types as public. Useful when preparing the library to be linked with tests project.

| Allowed value | Meaning
| --- | ---
| true | Translate internal members as public ones.
| false | Translate internal members as private ones, but generate 'friend' declaration for inter-class access if required.

**Default value**: false

### do_not_hardcode_aspose_cpp_path ###

Disables writing exact path to asposecpplib at CMakeLists.txt. Useful if converted project is compiled from directory different from the one it was translated into.

| Allowed value | Meaning
| --- | ---
| true | Do not put library path to CMakeLists.txt.
| false | Put library path to CMakeLists.txt.

**Default value**: false

### tools_version ###

Specific the version of tool that should be used on translating stage. Useful if project converted on machine with higher version of .NET Framework.

| Allowed value | Meaning
| --- | ---
| Tools version | Tools version recognized by MSBuild.

**Default value**: version of tool specified in project file or any available one if project file doesn't specify any.

### target_framework_version ###

Specific version of .NET Framework to use when parsing C# project.

| Allowed value | Meaning
| --- | ---
| .NET framework version | Framework available at "%ProgramFiles(x86)%\Reference Assemblies\Microsoft\Framework\.NETFramework\"

**Default value**: Version specified in project file.

### make_library ###

Alternative way to specify output library type in what relates to shared API macros.

| Allowed value | Meaning
| --- | ---
| shared | Create shared library
| static | Create static library
| api | Create library with API export macros. Use this if you are building e. g. shared library which consists of several static ones and need generate shared library exports when translating into static libraries.

**Default value**: static

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
| --- | --- | --- | --- | ---
| hide_local_symbols | Whether to avoid exporting private symbols. | true, false | false | false
| export_per_member | If true, export each member separately. If false, export whole classes. | true, false | false | true
| export_internals | If true, export internal members. | true, false | false | false
| shared_id | Export macro prefix | Identifier | false | Generated based on assembly name

### generate_includes_subdirectory ###

Creates a subdirectory under 'include' directory to avoid header name clashes when using several translated projects from single project.

| Allowed value | Meaning
| --- | ---
| false | Do not create subdirectory
| true | Create subdirectory named after the C# project unless the name is specified explicitly.

**Default value**: false

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
| --- | --- | --- | --- | ---
| directory | Explicit name of the subdirectory under 'include' folder. | String value | false | C# project name

### make_cpp_file_name_uniq ###

Controls translator behavior in whether file names should be unicalized by extending with trailing underscores.

| Allowed value | Meaning
| --- | ---
| true | All file names are unicalized.
| false | File names may repeat.

**Default value:** true

### headers_dir_name ###

Changes the directory where header files of a translated project will be stored. The 'include' directory name is used when this attribute is not present in the config file.

**Default value:** include

### sources_dir_name ###

Changes the directory where source files of a translated project will be stored. The 'source' directory name is used when this attribute is not present in the config file.

**Default value:** source

## Code readability ##

These options improve generated code's readability. However, the code generated now doesn't handle some corner cases properly or in the same way C# code does, so using these options on big codebases is error-prone. Instead, use them to translate e. g. code samples for your projects being translated, to make them easy to read.

### foreach_as_range_based_for_loop ###

Translate C# foreach loops as C++ [range-based for loops](https://en.cppreference.com/w/cpp/language/range-for)

```cs
foreach (HeaderFooter hf in doc.GetChildNodes(NodeType.HeaderFooter, true))
{
    // ...
}
```

| Allowed value | Meaning
| --- | ---
| false | Translate foreach loop as while loop
| true | Translate foreach loop as range-based for loop

False

```cpp
auto hf_enumerator = doc->GetChildNodes(NodeType::HeaderFooter, true)->GetEnumerator();
SharedPtr<HeaderFooter> hf;
while (hf_enumerator->MoveNext() && (hf = DynamicCast<HeaderFooter>(hf_enumerator->get_Current()), true))
{
    // ...
}
```

True

```cpp
for (auto hf : IterateOver<HeaderFooter>(doc->GetChildNodes(NodeType::HeaderFooter, true)) )
{
    // ...
}
```

**Default value**: false

### simplify_using_statements ###

Makes translator generate more compact code for 'using' statements that relies on used object destructors rather then on correct Dispose calls.

| Allowed value | Meaning
| --- | ---
| true | Do not generate compilcated code to call into Dispose().
| false | Generate correct Dispose calls anyway.

True

```cpp
{
    System::SharedPtr<Rs> __using_resource_0 = System::MakeObject<Rs>();
    System::Console::WriteLine(u"Statement");
}
```

False

```cpp
{
    System::SharedPtr<Rs> __using_resource_0 = System::MakeObject<Rs>();
    //Clearing resources under 'using' statement
    System::Details::DisposeGuard<1> dispose_guard_1({ using_resource_0});
   
    try
    {
        System::Console::WriteLine(u"Statement");
    }
    catch(...)
    {
        dispose_guard_1.SetCurrentException(std::current_exception());
    }
}
```

**Default value:** false

### force_auto_in_variable_declaration ###

Makes translator generate 'auto' types for local variables instead of full type name so that code is more compact.

| Allowed value | Meaning
| --- | ---
| true | Generate 'auto' type names.
| false | Generate full type names.

True

```cpp
auto rs = System::MakeObject<Rs>();
```

False

```cpp
System::SharedPtr<Rs> rs = System::MakeObject<Rs>();
```

**Default value:** false

### prefer_short_type_names ###

Makes translator prefer short type names where possible instead of fully qualified names in some contexts.

| Allowed value | Meaning
| --- | ---
| true | Use short names.
| false | Use fully qualified names.

True

```cpp
System::StaticCast<A>(o)
```

False

```cpp
System::StaticCast<Full::Namespace::Path::A>(o)
```

**Default value:** false

### use_stream_based_io ###

Replaces System::Console calls with cout invocations.

| Allowed value | Meaning
| --- | ---
| true | Switch to cout usage.
| false | Use fully qualified names.

True

```cpp
std::cout << "Hello" << std::endl;
```

False

```cpp
System::Console::WriteLn(u"Hello");
```

**Default value:** false

### generate_get_shared_members ###

Enables or disables generating GetSharedMembers() method for translated classes.

| Allowed value | Meaning
| --- | ---
| true | GetSharedMembers() method is generated.
| false | GetSharedMembers() method is not generated.

**Default value:** true

### generate_rtti_info ###

Enables or disables generating RTTI macros for translated classes.

| Allowed value | Meaning
| --- | ---
| true | RTTI macros are generated.
| false | RTTI macros are not generated.

**Default value:** true

## Code documentation ##

### keep_documentation_comments ###

Allows passing C# code documentation comments to C++ code.

| Allowed value | Meaning
| --- | ---
| true | Pass Doxygen-style comments to C++.
| false | Skip Doxygen-style comments.

**Default value**: false

### fix_self_closing_tags ###

Enables translator transforming self-closing documentation comment tags into pairs of opening and closing ones.

| Allowed value | Meaning | Description
| --- | --- | ---
| true | Transform self-closing tags. | \<tag\>' transforms into \<tag\>\</tag\>
| false | Keep self-closing tags as they are. | \<tag/\> remains as it is.

**Default value**: false

### try_expand_cref_types ###

Enables translator to replace cref types with proper C++ substitutions when translating documentation comments to Doxygen format.

| Allowed value | Meaning
| --- | ---
| true | Do the expansion of cref items.
| false | Do not expand cref items.

True

```xml
<see cref="Doxygen::GoldTests::Porter::TestClass"></see>
```

False

```xml
<see cref="TestClass"></see>
```

**Default value:** false

### hide_internal_declarations ###

Makes translator mark internal entities for Doxygen to skip them.

| Allowed value | Meaning
| --- | ---
| true | Make Doxygen skip internal entities.
| false | Make Doxygen generate documentation for internal entities.

True

```cpp
/// @cond
/// <summary>
/// internal constructor
/// </summary>
/// <param name="value"></param>
AbstractTestClass(uint8_t value);
/// @endcond
```

False

```cpp
/// <summary>
/// internal constructor
/// </summary>
/// <param name="value"></param>
AbstractTestClass(uint8_t value);
```

**Default value:** false

### hide_friend_declarations ###

Makes the translator to generate the '@cond...@endcond' wrappers around friend declarations to exclude them from the Doxygen documentation.

| Allowed value | Meaning
| --- | ---
| true | Generate wrappers.
| false | Do not generate wrappers.

True

```cpp
/// @cond
friend class MyClass;
/// @endcond
```

False

```cpp
friend class MyClass;
```

**Default value:** false

### remove_private_comments ###

Makes translator remove comments for private entities.

| Allowed value | Meaning
| --- | ---
| true | Translate comments for non-private entities only.
| false | Translate comments for all entities.

**Default value:** false

## Redundant options ##

Options no longer supported but still recognized (and ignored) by translator for compatibility reasons are:

* alternative_base
* boost_ver
* additional_libdirs
* singleton_mode
* auto_weak_ptr_reference
* gtest_path
* insert_using_statement_guard
* using_statement_as_lambda
* using_statement_enhanced
* avoid_lambda_holders_if_possible
* log_level
* abort_on_error
* write_progress
* collect_porter_coverage_info
* build_cs_projects
* start_block_newline
* use_pragma_once
* replace_wchar_with_hex_literal
* obfuscate_cpp_headers
* force_include_enum
* use_full_base_name
* deprecate_system_base_type
* ignore_base_for_static_class
* replace_enumerable_type
* remove_redundant_base_interfaces
* enable_fast_rtti
* exclude_by_description
* emit_enumerator_current_value_holder
* create_unit_test_preprocessor_directive
* generate_abstract_keyword
* add_baseclasses_tests
* generate_enum_descriptions
* rtti_on_testfixture

## Notes ##

Code examples used on this page are for illustration purposes only. Actual translator output may differ.
