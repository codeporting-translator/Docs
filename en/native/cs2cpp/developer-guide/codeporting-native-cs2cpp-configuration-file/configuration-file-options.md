---
date: "2020-01-23"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Configuration file Options"
linktitle: "Configuration file Options"
description: "List of all Options available in Configuration File"
menu:
  docs:
    parent: "CodePorting.Native Cs2Cpp Configuration File"
    weight: "4"
lastmod: "2020-01-23"
weight: "4"
---

## Configuration file Options ##

There is a number of options that can be used with porter config. The general syntax to add an option is as follows:

{{< highlight xml >}}

<porter>
    <opt name="option_name_1" value="option_value_1"/>
    <opt name="option_name_2" value="option_value_2" attribute1="attribute1_value"/>
    <opt name="option_name_3">Some text</opt>
    ...
</porter>
{{< /highlight >}}

The options can be split into several categories. This page presents full list of available options.

{{<note>}}
Code examples used on this page are for illustration purposes only. Actual porting application output may differ.
{{</note>}}

### General workflow options ###

These options define general porter behavior: logging, errors handling, etc.

#### log_level ####

Defines cut level of logs being written by porting application. Logs of specified level and more severe ones are emitted, the logs with lower severity are suppressed.

| Allowed value | Meaning
---| ---|
| debug | Debug logs
| info  | Logs that do not indicate errors
| warning | Logs showing potential problem
| error | Logs indicating error condition


**Default value**: debug

#### abort_on_error ####

States whether the porting application should abort execution if any error is encountered. Normally, some errors are not fatal.

| Allowed value | Meaning
---| ---|
| true | Abort on error
| false |Do not abort on error


**Default value**: false

#### write_progress ####

Whether to display line with completion percentage during the work.

| Allowed value | Meaning
---| ---|
| true | Show percentage
| false | Do not show percentage


**Default value**: true

### Output files control options ###

These options control how output files are generated.

#### compare_cpp_hash ####

Allows it to skip overwriting the ported cpp and h files if the hash of their contents already matches such of the ported code to be saved here. Useful if you're frequently changing and porting C# code without cleaning the output directory and want to avoid recompiling all C++ files (overwriting the same contents updates file timestamp so that the build system will mark it as changed even if the contents is same).

| Allowed value | Meaning
---| ---|
| true | Do not overwrite files if hashes match
| false | Overwrite files unconditionally


**Default value**: true

#### write_bom ####

Whether to add BOM record at the beginning of each output C++ file.

| Allowed value | Meaning
---| ---|
| true | Add BOM
| false | Do not add BOM


**Default value**: false

#### write_include_map ####

States whether to create include map file which then can be used to manage includes from the dependent project. If you plan to port another project which depends on the current one, creating typemap is the safest way to share type information between them.

| Allowed value | Meaning
---| ---|
| true | Create typemap
| false | Do not create typemap


**Default value**: true

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
---| ---| ---| ---| ---|
| public_only | Whether to exclude non-public types from the map | * **true** - dump public types only * **false** # dump all files | No | true
| with_dir_prefix | Whether to use full (with directory prefix) includes or project-local ones | * **true** - write full includes * **false** - write local includes | No | true

#### tab ####

Indent substitution. You can use '\n' and '\t' references there as well as other in-line characters.

| Allowed value | Meaning
---| ---|
| String value | Exact string to define one level of indentation


**Default value**: Four space characters

#### endl ####

Line end substitution. You can use '\n', '\r' and '\t' references there.

| Allowed value | Meaning
---| ---|
| String value | Exact string to define line break


**Default value**: '\n' aka line break

#### start_block_newline ####

Whether the opening curl bracket beholds on a dedicated line.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Opening curl bracket resides on dedicated line | {{< highlight xml >}}
if (++a == 10)
{
    a = 0;
    ++b;
}
{{< /highlight >}}
| false | Opening curl bracket resides at the end of current line | {{< highlight xml >}}
if (++a == 10) {
    a = 0;
    ++b;
}
{{< /highlight >}}


**Default value**: true

#### low_case_file_names ####

Whether output file names are all lowercase. In this mode, word borders in Camel case class names become underscores.

| Allowed value | Meaning | Example
---| ---| ---|
| true | All output file names are in lower case. | 'MyNewClass' class resides in header file called 'my_new_class.h'
| false | All output file names keep case of original type name. | 'MyNewClass' class resides in header  file called 'MyNewClass.h'


**Default value**: false

#### use_pragma_once ####

Whether to use '#pragma once' instead of scope ifndef.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Use 'pragma once' instruction. | {{< highlight cpp >}}
#pragma one

#include <cstdint>

namespace SampleProject {

enum class Enum: uint8_t
{
    item1,
    item2,
    item3
};

} // namespace SampleProject
{{< /highlight >}}
| false | Use 'ifdef guard' construct | {{< highlight cpp >}}
#ifndef _SampleProject_enum_h_
#define _SampleProject_enum_h_

#include <cstdint>

namespace SampleProject {

enum class Enum: uint8_t
{
    item1,
    item2,
    item3
};

} // namespace SampleProject

#endif // _SampleCsProject_enum_h_
{{< /highlight >}}


**Default value**: true

#### replace_wchar_with_hex_literal ####

Whether to replace multibyte symbols in string literals with the hex literals.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Replace multibyte symbols with hex literals. | {{< highlight cpp >}}
string a = u"\u0430\u0431\u0432";
{{< /highlight >}}
| false | Keep multibyte symbols as they are. | {{< highlight cpp >}}
string a = u"абв";
{{< /highlight >}}

**Default value**: false

#### force_public_headers ####

Enables integrity support for public header files.

| Allowed value | Meaning | Example
---| ---| ---|
|true|Checks that all header files included in public header files are in the 'include /' directory. If not, then transfers them into it.|
|false|Header files included in the public header files remain in the 'source /' directory.|

**Default value**: false

### Type subsystem options ###

These options define how porter behaves regarding the translation of the type.

#### forwarding_if_possible ####

Use forward declarations in header files instead of includes if possible. This affects function parameters and returns values declarations as well as pointers. This does not impact inheritance or value type field cases.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Use forward declaration instead of include. | {{< highlight cpp >}}
namespace MyProject {
class A;
class B
{
public:
    System::SmartPtr<A> Value;
};
} // namespace MyProject
{{< /highlight >}}
| false | Use include instead of forward declaration. | {{< highlight cpp >}}
#include "classes/A.h"
namespace MyProject {
class B
{
public:
    System::SmartPtr<A> Value;
};
} // namespace MyProject
{{< /highlight >}}


**Default value**: false

#### force_include_enum ####

Use includes instead of forward declarations in header files for enums. Changing the **forwarding_if_possible** option does not affect this behavior.

| Allowed value | Meaning | Example
---| ---| ---|
|true |Use include instead of a forward declaration. | |  {{< highlight cpp >}}
#include "enums/A.h"

namespace MyProject {

class B

{

public:

    System::SmartPtr<A> Value;

};

} ~/~/ namespace MyProject
{{< /highlight >}}
| false|Use forward declaration instead of include. | | {{< highlight cpp >}}
namespace MyProject {

enum class A;

class B

{

public:

    System::SmartPtr<A> Value;

};

} ~/~/ namespace MyProject
{{< /highlight >}}

**Default value**: false

#### use_full_base_name ####

When referring base class, use the full class name (with all namespaces) instead of the short name.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Use full base name. | {{< highlight cpp >}}
namespace MyProject {
class A
{
    ...
};
class B : public MyProject::A
{
    ...
};
} // namespace MyProject
{{< /highlight >}}
| false | Use compact base name. | {{< highlight cpp >}}
namespace MyProject {
class A
{
    ...
};
class B : public A
{
    ...
};
} // namespace MyProject
{{< /highlight >}}

**Default value**: false

#### external_object_methods ####

Use static implementations of 'ToString', 'GetType', 'GetHashCode', 'Equals' methods and 'is' operator located in ObjectExt class instead of the ones provided by Object class itself. Try this option if you experience problems with these methods being called for primitive types or generic type parameters or alike.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Use external object methods. | {{< highlight cpp >}}
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
{{< /highlight >}}
| false | Use in-object methods. | {{< highlight cpp >}}
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
{{< /highlight >}}

**Default value**: false

#### cast_delegate ####

When is a method parameter, cast delegate function to the actual type of expected parameter. Helps to resolve ambiguity in some cases (in C++, lambdas are not of corresponding MulticastDelegate type which can result in ambiguity if more than one signature exists).

| Allowed value | Meaning | Example
---| ---| ---|
| true | Add cast expression. | {{< highlight cpp >}}
ApplyDelegate(static_cast<typename DelegateType>([](String s) { return s; });
{{< /highlight >}}
| false | Do not add cast expression. | {{< highlight cpp >}}
ApplyDelegate([](String s) { return s; });
{{< /highlight >}}

**Default value**: true

### deprecate_system_base_type ####

Whether to omit baseclass references for the classes from 'System' or 'Microsoft' namespaces. Use with care as it affects the whole project. If in doubt, consider using CppIgnoreBaseType attribute instead.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Do not inherit from several base classes. | {{< highlight cpp >}}
class MyStream
{
    ...
};
{{< /highlight >}}
| false | Inherit from any base class. | {{< highlight cpp >}}
class MyStream : public System::IO::Stream
{
    ...
};
{{< /highlight >}}


**Default value**: false

#### exception_as_reference ####

Whether to pass exceptions to functions by reference rather than by creating a local copy of an exception object.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Pass exceptions as references. | class MyClass {{< highlight cpp >}}
{
public:
    void HandleException(Exception &e);
};
{{< /highlight >}}

|
false
|
Pass exceptions by value.
|
{{< highlight cpp >}}
class MyClass
{
public:
    void HandleException(Exception e);
};
{{< /highlight >}}

**Default value**: false

#### ignore_base_for_static_class ####

Whether to omit 'Object' baseclass for the classes declared as static.

{{< highlight cpp >}}
static class MyClass
{
    ...
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Do not inherit static classes from Object. | {{< highlight cpp >}}
class MyClass
{
    ...
};
{{< /highlight >}}

|
false
|
Inherit static classes from Object.
|
{{< highlight cpp >}}
class MyClass : public System::Object
{
    ...
};
{{< /highlight >}}


**Default value**: true

#### replace_enumerable_type ####

{{< highlight xml >}}
<opt name="replace_enumerable_type" enumerable="System.Xml.XmlNodeList" type="System.Xml.XmlNode"/>
{{< /highlight >}}

Adds a enumerable type that needs downcasting when in loop operations. Use this option to specify explicitly which type downcasts into what.

| Allowed value | Meaning | Example
---| ---| ---|

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
---| ---| ---| ---| ---|
| enumerable | Original type | Enumerable type full name | Yes |
| type | Type to cast to | Target type full name | Yes |


#### force_dynamic_cast ####

{{< highlight xml >}}
<opt name="force_dynamic_cast" from_type="A.B.ClassA" to_type="A.B.ClassB" />
{{< /highlight >}}

For two given types, forces dynamic_cast to be used instead of default static_cast. Use if auto cast type deduction fails (e. g. due to diamond problem).

| Allowed value | Meaning | Example
---| ---| ---|

| Additional attribute | Meaning | Allowed values | Mandatory | Default value
---| ---| ---| ---| ---|
| from_type | Source type | Full type name | Yes |
| to_type | Target type | Full type name | Yes |


#### remove_redundant_base_interfaces ####

If enabled, removes redundant inheritance from interface types, i. e. interfaces inherited more than once.

{{< highlight cpp >}}
namespace TypesPorting
{
    public interface IFoo
    {
    }

    public abstract class AFoo : IFoo
    {
    }

    public class Foo : AFoo, IFoo
    {
    }

    public class Bar : Foo, IFoo
    {
    }
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Remove redundant interfaces | {{< highlight cpp >}}
class IFoo : public System::Object
{
    ...
};
class ABSTRACT AFoo : public IFoo
{
    ...
};
class Foo : public AFoo
{
    ...
};
class Bar : public Foo
{
    ...
};
{{< /highlight >}}
| false | Do not remove redundant interfaces | {{< highlight cpp >}}
class IFoo : public System::Object
{
    ...
};
class ABSTRACT AFoo : public IFoo
{
    ...
};
class Foo : public AFoo, public IFoo
{
    ...
};
class Bar : public Foo, public IFoo
{
    ...
};
{{< /highlight >}}


**Default value**: false

### C# code analysis options ###

These options impact how the original C# code gets analyzed

#### exclude_by_description ####

Allows it to exclude the entities from porting based on description comment. Individual description comment values can be provided as text inside 'text' subnodes.

Example:

{{< highlight xml >}}
<opt name="exclude_by_description">
    <text>This is for COM compatibility.</text>
    <text>For COM compatibility.</text>
</opt>
{{< /highlight >}}

### C++ code generation parameters ###

These options define how porter uses specific C++ code features.

### detect_const_methods ####

Whether to generate 'const' specifier on methods that do not modify their object. These can be either marked with CppConstMethod attribute or found const by porter check. Therefore, if this option is false, CppConstMethod attribute has no effect.

{{< highlight cpp >}}
class Foo
{
    public void Bar() {}
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Mark methods as const | {{< highlight cpp >}}
class Foo
{
    ...
    void Bar() const;
};
{{< /highlight >}}
| false | Do not mark methods as const | {{< highlight cpp >}}
class Foo
{
    ...
    void Bar();
};
{{< /highlight >}}


**Default value**: false

#### exclude_volatile ####

Whether to pass 'volatile' flag from C# to C++.

{{< highlight cpp >}}
class Foo
{
    private volatile int m_bar;
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Mark members as volatile | {{< highlight cpp >}}
class Foo
{
    ...
    volatile int m_bar;
};
{{< /highlight >}}
| false | Do not mark members as volatile | {{< highlight cpp >}}
class Foo
{
    ...
    int m_bar;
};
{{< /highlight >}}


**Default value**: false

#### put_enum_on_top ####

Whether enum declarations preceed class and struct ones in output header files.

{{< highlight cpp >}}
class A {}
enum B { C, D };
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Enums are translated first | {{< highlight cpp >}}
enum class B { C, D };
class A { ... };
{{< /highlight >}}
| true | Order is unchanged | {{< highlight cpp >}}
class A { ... };
enum class B { C, D };
{{< /highlight >}}


**Default value**: false

#### alternative_string_switch ####

Define whether to use if-else or do-while form of string switch translation.

{{< highlight cpp >}}
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
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Use do-while form. | {{< highlight cpp >}}
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
{{< /highlight >}}
| false | Use if-else form. | {{< highlight cpp >}}
if (s == u"abc")
{
    s2 = u"cba";
}
else if (s == u"123")
{
    s2 = u"321";
}
{{< /highlight >}}


**Default value**: false

#### alternative_null_coalescing ####

Use an alternative form of '??' operator translation which avoids it calculating right-hand operand unless it is used.

{{< highlight cpp >}}
Object obj = obj1 ?? new Object();
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Use alternative form. | {{< highlight cpp >}}
System::SharedPtr<System::Object> obj = System::ObjectExt::Coalesce(obj1, [&](){return System::MakeObject<System::Object>();});
{{< /highlight >}}
| false | Use usual form. | {{< highlight cpp >}}
System::SharedPtr<System::Object> obj = obj1 != nullptr ? obj1 : System::MakeObject<System::Object>();
{{< /highlight >}}


**Default value**: false

#### remove_unused_namespaces ####

Remove unused 'using namespace' directives (the references to namespaces no classes from which ones are used). Such constructs can result in compilation errors: if the classes from the namespace are not used, it is possible that no includes introducing this namespace exist, so the name does not get recognized by the compiler.

{{< highlight cpp >}}
using System;
using System.Collections.Generic;
class MyClass
{
    void Foo()
    {
        BitConverter.GetBytes(123);
    }
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Remove unused namespaces | {{< highlight cpp >}}
#include "MyClass.h"

using namespace System;

void MyClass::Foo()
{
    BitConverter::GetBytes(123);
}
{{< /highlight >}}
| false | Keep unused namespaces | {{< highlight cpp >}}
#include "MyClass.h"

using namespace System;
using namespace System::Collections::Generic;

void MyClass::Foo()
{
    BitConverter::GetBytes(123);
}
{{< /highlight >}}


**Default value**: false

#### indexer_as_method ####

Defines whether to translate indexer invocation as method instead of operator [] even if the later form is possible.

{{< highlight cpp >}}
System.Collections.Generic.List<int> mylist = GetList();
int i = mylist[0];
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Translate indexers as methods | {{< highlight cpp >}}
System::Collections::Generic::ListPtr<int> mylist = GetList();
int i = mylist->idx_get(0);
{{< /highlight >}}
| false | Translate indexers as operators | {{< highlight cpp >}}
System::Collections::Generic::ListPtr<int> mylist = GetList();
int i = mylist[0];
{{< /highlight >}}


**Default value**: true

#### create_unit_test_preprocessor_directive ####

Whether to pass '#ifdef UNIT_TEST' directives from C# to C++.

{{< highlight cpp >}}
#ifdef UNIT_TEST
[NUnit.Framework.TestFixture]
class MyTests { ... }
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Pass ifdef macros to C++. | {{< highlight cpp >}}
#ifdef UNIT_TEST
class MyTests : public System::Object, public ::testing::Test
{
    ...
};
#endif
{{< /highlight >}}
| false | Skip ifdef macros. | {{< highlight cpp >}}
class MyTests : public System::Object, public ::testing::Test
{
    ...
};
{{< /highlight >}}


**Default value**: false

#### generate_abstract_keyword ####

Whether to add an 'abstract' attribute to abstract classes. (Currently it is being inserted as 'ABSTRACT' define rather than as native C++11 keyword.)

{{< highlight cpp >}}
abstract class Abstract
{
    ...
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Generate ABSTRACT labels. | {{< highlight cpp >}}
class ABSTRACT Abstract : public System::Object
{
    ...
}
{{< /highlight >}}
| false | Omit ABSTRACT labels. | {{< highlight cpp >}}
class Abstract : public System::Object
{
    ...
}
{{< /highlight >}}


**Default value**: true

#### use_weak_ptr_std_bind ####

When generating std::bind() expressions for delegates porting, use WeakPtr instead of raw C++ pointers to pass object reference.

{{< highlight cpp >}}
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
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Use WeakPtrs instead of raw C++ pointers. | {{< highlight cpp >}}
ModifyString myDelegate = std::bind(&WithDelegate::AddPrefix, System::WeakPtr<WithDelegate>(this), std::placeholders::_1);
{{< /highlight >}}
| false | Use raw C++ pointers. | {{< highlight cpp >}}
ModifyString myDelegate = std::bind(&WithDelegate::AddPrefix, this, std::placeholders::_1);
{{< /highlight >}}


**Default value**: false

#### deferred_init ####

If enabled, replaces all static fields with singletons and calls static constructors from instance constructors and singleton access functions instead of C++ static objects initializers. Use this option to resolve the static objects initialization races. Slows down static fields access and constructors as there are additional checks.

| Allowed value | Meaning | Example
---| ---| ---|
| None | Disabled | Static constructors are ported as constructors of global static variables. Static class fields are ported as static class fields.
| All | Enabled for all classes | Static constructors are ported as static functions. Static class fields are ported as singletons. Constructors and singleton accessors call into static constructor to make sure it is finished before object creation or static variable access.
| Tests  | Enabled for test classes only | Static constructors of TestFixture classes are ported as static functions. TestFixture classes static fields are ported as singletons. Constructors and singleton accessors of TestFixture classes call into static constructor to make sure it is finished before object creation or static variable access.
Static constructors of non-TestFixture classes are ported as constructors of global static variables. Static fields of non-TestFixture classes are ported as static class fields.


**Default value**: None

#### auto_ctor_self_reference ####

If enabled, puts constructor self-reference guards where required, allowing it for constructor to refer to 'this' without deleting the object. Saves the developer from putting CppCtroSelfReference attributes manually but creates more guards than actually required.

{{< highlight cpp >}}
class MyClass
{
    public MyClass()
    {
        SomeOtherClass.DoSomething(this);
    }
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Place guards that allow safe usage pf shared pointers to object being constructed | {{< highlight cpp >}}
MyClass::MyClass()
{
    IncSelfReference();
    auto __local_self_ref = System::MakeScopeGuard([this]{ DecSelfReference(); });
    SomeOtherClass::DoSomething(System::MakeSharedPtr(this));
}
{{< /highlight >}}
| false | Do not place guards automatically. Make sure to use CppCtroSelfReference attributes manually, otherwise you will have a 'deletion in constructor' issue. | {{< highlight cpp >}}
MyClass::MyClass()
{
    SomeOtherClass::DoSomething(System::MakeSharedPtr(this));
}
{{< /highlight >}}


**Default value**: false

#### force_add_shared_api_macros ####

If enabled, forces production of shared_api_defs.h file and inserts corresponding macros into the ported code. This helps to switch between shared and static library project using the make_shared_lib option but without re-porting the whole project.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Create shared_api_defs.h file regardless which library type (shared or dynamic) is targetted |
| false | Create shared_api_defs.h file only if targetting shared library. |



**Default value**: false

#### finally_statement_as_lambda ####

Allows porting the try-finally statement as a lambda expression instead of guard object placement.

| Allowed value | Meaning | Example
---| ---| ---|
| true | try-finally statement is translated through lambdas. |
| false | try-finally statement is translated using sentry object. |



**Default value**: false

#### setter_wrap_with_lambda ####

Forces translating complex property assignment operators using lambdas.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Complex property assignments use lambdas. |
| false | Complex property assignments are translated using default approach. |



**Default value**: false

#### allow_interface_members_base_class_impl ####

In C#, members of interface can be implemented in the base class. In C#, there's no way doing so. This option generates required calls in child class; however, this can overcomplicate output code in some cases.

{{< highlight cpp >}}
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
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Adds required calls to methods implemented in base classes. | {{< highlight cpp >}}
class Foo : public FooImpl, public IFoo
{
    ...
    void Do(int32_t i, System::String s);
};
void Foo::Do(int32_t i, System::String s)
{
    FooImpl::Do(i, s);
}
{{< /highlight >}}
| false | Doesn't generate required calls. |



**Default value**: false

#### polymorphic_memberwiseclone ####

{{< highlight xml >}}
<opt name="polymorphic_memberwiseclone" value="true">
    <root class="MyNamespace.MyClass"/>
    <root class="MyNamespace.MyClass2"/>
</opt>
{{< /highlight >}}

By default, MemberwiseClone() method in ported code slices output object to the type it is called for. All information about child classes is lost for all implementation is static. This option allows injecting additional virtual methods to the classes MemberwiseClone() is called for and to their child classes. This fixes MemberwiseClone() behavior, but generates additional code. Please note that the porting application only considers MemberwiseClone() calls located in same assembly by default and doesn't generate additional code for classes that are not subjects for MemberwiseClone() calls. To force-generating these methods for specific classes and their subclasses (e. g. if MemberwiseClone() is called from different assembly), use 'root' subnodes with mandatory 'class' attributes containing C# class names.

| Allowed value | Meaning | Example
---| ---| ---|
| true | MemberwiseClone() clones full class tree. |
| false | MemberwiseClone() cuts class tree being copied up to the class it is called upon. |



**Default value**: false

#### version_compatibility_check_mode ####

Allows porting application to generate code which compares headers version used to compile project and supplied library version on startup.

| Allowed value | Meaning | Example
---| ---| ---|
| stderr | On version mismatch, write error message to stderr, add a record to modules version mismatch registry and continue |
| stdout | On version mismatch, write error message to stdout, add record to modules version mismatch registry and continue |
| silent | No output; on version mismatch, add a record to modules version mismatch registry and continue |
| exit | On version mismatch, call std::exit(EXIT_FAILURE) |
| none | Don't add version check code to the resulting project |



**Default value**: stderr

#### force_const_auto_property_getter ####

Marks auto-generated property getters as const methods.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Mark auto-generated property getters as const. |
| false | Do not mark auto-generated property getters const. |


**Default value**: false

#### force_const_simple_property_getter ####

Marks simple property getters consisting of single 'return field_name' statement as const.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Mark property getters like 'return field_name' as const. |
| false | Keep such property getters non-const. |



**Default value**: false

#### keep_documentation_comments ####

Allows passing C# code documentation comments to C++ code.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Pass Doxygen-style comments to C++. |
|  false | Skip Doxygen-style comments. |



**Default value**: false

#### process_base_overloading ####

Puts 'using' statement to re-declare hidden baseclass methods in subclasses.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Add using statements | {{< highlight cpp >}}
class Bar {
    ...
    void Do();
};
class Foo : public Bar {
    ...
    void Do(System::String s);
    using Bar::Do;
};
{{< /highlight >}}
| false | Do not add using statements | {{< highlight cpp >}}
class Bar {
    ...
    void Do();
};
class Foo : public Bar {
    ...
    void Do(System::String s);
};
{{< /highlight >}}


**Default value**: false

#### thread_static_generation ####

Determines how to translate ThreadStatic attribute.

| Allowed value | Meaning | Example
---| ---| ---|
| disabled | Ignore ThreadStatic attribute | {{< highlight cpp >}}
static System::String m_value;
{{< /highlight >}}
|  native | Translate ThreadStatic attribute as thread_local storage class. | {{< highlight cpp >}}
static thread_local System::String m_value;
{{< /highlight >}}


**Default value**: native

#### remove_inactive_code ####

Drops comments with inactive code.

| Allowed value | Meaning | Example
---| ---| ---|
| false | Keep comments with inactive code. |
| true | Drop inactive code silently. |


**Default value**: false

#### emit_preprocessor_directives ####

Propagates preprocessor directives to C++.

| Allowed value | Meaning | Example
---| ---| ---|
| false | Drop preprocessor directives. |
|  true | Add comments on used preprocessor directives. |



**Default value**: true

#### emplace_assembly_details ####

Enables passing assembly details (assembly name, etc.) to ported code.

| Allowed value | Meaning | Example
---| ---| ---|
|  false | Do not pass assembly info. |
| true |Pass assembly info. |



**Default value**: false

#### foreach_as_range_based_for_loop ####

Translate C# foreach loops as C++ [range-based for loops](https://en.cppreference.com/w/cpp/language/range-for)

{{< highlight cpp >}}
foreach (HeaderFooter hf in doc.GetChildNodes(NodeType.HeaderFooter, true))
{
    // ...
}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| false| Translate foreach loop as while loop | {{< highlight cpp >}}
{{{auto hf_enumerator = doc->GetChildNodes(NodeType::HeaderFooter, true)->GetEnumerator(); SharedPtr<HeaderFooter> hf; while (hf_enumerator->MoveNext() && (hf # DynamicCast<HeaderFooter>(hf_enumerator->get_Current()), true)) { // ... }}}}
{{< /highlight >}}
| true | Translate foreach loop as range-based for loop | {{< highlight cpp >}}
{{{for (auto hf : IterateOver<HeaderFooter>(doc->GetChildNodes(NodeType::HeaderFooter, true)) ) { // ... }}}}
{{< /highlight >}}

**Default value**: false

#### add_baseclasses_tests ####

If the class not marked with NUnit.Framework.TestFixture attribute contains methods maked with NUnit.Framework.Test attribute and is inherited by a class marked with NUnit.Framework.TestFixture attribute, the gtest tests are generated for child class instead of parent class.

{{< highlight cpp >}}
class FixtureBase {
    [Test]
    public void MyTest() {}
}
[TestFixture]
class Fixture : FixtureBase
{}
{{< /highlight >}}

| Allowed value | Meaning | Example
---| ---| ---|
| true | Move tests to child class | {{< highlight cpp >}}
{{{TEST_F(Fixture, MyTest) { ... }}}}
{{< /highlight >}}
| false | Generate tests to base class | {{< highlight cpp >}}
{{{TEST_F(FixtureBase, MyTest) { ... }}}}
{{< /highlight >}}

**Default value**: true

#### nunit_assert_class_aliases ####

Calls to methods of NUnit.Framework.Assert class are translated into gtest-compatible macros. If you use your own test class with similarly named methods, use this option to enable special treatment of these. Enlist classes wrapped inside 'alias' subnode as shown below. Also, you might want to exclude such classes from porting.

|
{{< highlight xml >}}
<opt name="nunit_assert_class_aliases" value="true">

    <alias class="MyNamespace.MyAssertClass"/>

</opt>
{{< /highlight >}}


| Allowed value | Meaning | Example
---| ---| ---|
|true|Allow Assert-like classes special treatment.|
|false|Translate Assert-like classes as usual.|

**Default value**: false

#### original_tests_names ####

Toggles prefixing test name with category name to simplify tests group run after porting. Alternatively, if you prefer having original tests names, you might want to disable this option.


{{< highlight xml >}}
<opt name="original_tests_names" value="true"/>
{{< /highlight >}}

{{< highlight cpp >}}
[TestFixture]

public class OriginalTestName

{

    [Test]

    [Category("Original")]

    public void Test1() {}

}
{{< /highlight >}}


| Allowed value | Meaning | Example
---| ---| ---|
| true| Do not add category prefix.|  {{< highlight cpp >}}
TEST_F(OriginalTestName, Test1) { s_instance->Test1(); }
{{< /highlight >}}
| false | Add category prefix. | {{< highlight cpp >}}
TEST_F(OriginalTestName, Original_Test1) { s_instance->Test1(); }
{{< /highlight >}}



**Default value**: false

#### cpp_enum_enable_metadata ####

Enables metadata globally, same as [CppEnumEnableMetadata](https://wiki.csporter.com/cpp/Developer%20Guide/csPorter%20for%20Cpp%20Attributes/#HCppEnumEnableMetadata) attribute does for individual enums.

{{< highlight xml >}}
<opt name="cpp_enum_enable_metadata" value="true"/>
{{< /highlight >}}


| Allowed value | Meaning | Example
---| ---| ---|
|true|Generate metadata for all enums.|Enum to string and string to enum conversions provide full text information, same as in C#.
|false|Generate metadata for enums marked with CppEnumEnableMetadata attribute only.|Unmarked enums convert to string and from string in numeric format only.

**Default value**: false


#### generate_enum_descriptions ####

Enables passing System.ComponentModel.Description attribute value to C++ code.

{{< highlight xml >}}
<opt name="generate_enum_descriptions" value="true"/>
{{< /highlight >}}

To extract such data, use the following syntax:

{{< highlight xml >}}
System::Enum<T>::GetDescription(value)
{{< /highlight >}}

Here **T** is enum type and **value** is enum value.

| Allowed value | Meaning | Example
---| ---| ---|
|true|Pass attribute values to C++.|
|false|Ignore attribute values.|

**Default value**: false

#### fix_self_closing_tags ####

Enables porter transforming self-closing documentation comment tags into pairs of opening and closing ones.

| Allowed value | Meaning | Example
---| ---| ---|
|true|Transform self-closing tags.|'<tag/>' transforms into '<tag></tag>'
|false|Keep self-closing tags as they are.|'<tag/>' remains as it is.

**Default value**: true
**Since version**: 20.1

### Debug and developer version code options ###

These options controls debug and developer version code in generated C++ files.

#### collect_test_methods ####

Whether to collect information on ported code test methods in C++ runtime by calling "System::TestToolsExt::RegisterTest()" for each test method on initialization stage.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Collect information on tests |
| false | Don't collect information on tests |

**Default value**: false

#### generate_for_each_member ####

Enables adding for_each_member subsystem-related code to each class and generating gv (graphviz) dumps of in-memory objects after each test.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Generate for_each_member-related code |
| false | Don't generate for_each_member-related code |



**Default value**: false

#### stable_gv_file ####

Enables renumbering objects in for_each_member-based gv dumps so that the indexes in output files do not depend on any temporary objects being created and then destroyed during the execution. Required if you want to compare the gv file against a template and want it to remain stable in all builds (release and debug builds and Visual Studio and non-Visual Studio builds currently create different amounts of temporary objects, therefore, affecting object indexes in non-stabilized builds).

| Allowed value | Meaning | Example
---| ---| ---|
| true | Renumbers objects when dumping gv files to stabilize them. |
| false | Disables objects renumbering. |


**Default value**: false

#### test_run_stub_file ####

Creates a file to dump all tests names into during porting

| Allowed value | Meaning | Example
---| ---| ---|
| <Path to stub file> | Path to the file to enlist all tests. |
| MyTests.txt  | <Empty> | Disables tests enlisting. |



**Default value**: <Empty>

#### insert_leakage_detectors ####

Insert the helper code to detect the constructors leaking in references. This usually means that the nested objects created by this constructor refer to the object itself using shared pointers instead of weak ones which promise some big problems (memory leaks, double deletion issues on constructor exceptions, etc.). If enabling this feature, check output in debugging to track potential problems.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Insert leakage detection code. | Example output message:  {{< highlight cpp >}}
Shared pointer leakage: constructor MyClass::MyClass(int, int) leaked 7 references.
{{< /highlight >}}
| false | Doesn't generage code to do the checks. |



**Default value**: false

#### tests_garbage_collection ####

Calls ~_~_DBG_GARBAGE_COLLECTION mechanism after each test.

| Allowed value | Meaning | Example
---| ---| ---|
| none | Doesn't collect garbage after tests
| report | Collects garbage and reports collected objects after each test. | {{< highlight cpp >}}
Island of isolation is found.
Objects:
    0x11223344: MyClass: 2 reference
        m_a: 0x55667788
        m_b: 0x99aabbcc
    0x55667799: MyClass2: 1 reference
        m_owner: 0x11223344
    0x99aabbcc: MyClass2: 1 reference
        m_owner: 0x11223344
{{< /highlight >}}
As one can see, class names, memory addresses, member names and addresses of objects these members point to are listed.
| free |Report collected objects and delete them afterwards. |



**Default value**: none

#### tests_garbage_collection_generation ####

GC generation to collect by ~_~_DBG_GARBAGE_COLLECTION wrappers after tests. For optimization purposes.

| Allowed value | Meaning | Example
---| ---| ---|
| Integer value 0 to 2 | Same as generation used by GC in C# |


**Default value**: 0

#### add_category_name_to_timeout_tests ####

If defined, value is used as a category name for all tests with timeouts.

**Default value**: <Not defined>

### Path resolving behavior ###

This option regulate how porter check path that located in the config file.

#### use_porter_home_directory_while_resolving_path ####

Enables porter using porter home directory and porter executable location when resolving paths mentioned in configuration file. Only affects declarations that go after it in configuration file. Can't be disabled if it is enabled somewhere else in configuration file.

| Allowed value | Meaning | Example
---| ---| ---|
|false|Only directory with current configuration file is used as a lookup for mentioned paths.|
|true|Adds porting application binary location and %PorterHome% variable set either explicitly or via porting application command line to the list of lookup directories.|

**Default value**: false

#### Project settings ####

These options specify the settings of output project.

#### cmake_targets ####

Whether to build ported project, tests or both.

| Allowed value | Meaning | Example
---| ---| ---|
|  PortedProject | Only build ported project (application if source project is application, library if source project is library) |
|  Tests | Only build test application but not the ported project |
| Both | Build both tests and ported project |


**Default value**: Both

#### make_shared_lib ####

Whether to generate a shared library project or static library project. Only makes effect if building a ported project is allowed (see cmake_targets option) and source project ls a library rather than executable.

| Allowed value | Meaning | Example
---| ---| ---|
|  false | Generate static library |
| true | Generate shared library |



**Default value**: false

| Additional attribute |  Meaning |  Allowed values | Mandatory |  Default value  
---| ---| ---| ---| ---|
| export_per_member | Whether to generate per-member export attributes instead of per-class ones. true false | No  | true
| export_internals | Whether to add *SHARED_API macro to internal class members, not only to public class members. | true false | No | false
| shared_id | Overrides default (generated) *SHARED_API macro. | Prefix to *_SHARED macro. | No | Assembly name with dots replaced with underscores

#### cpp_lib_path ####

Path to system library folder (the one containing 'include/' and 'lib/' directories).

| Allowed value | Meaning | Example
---| ---| ---|
|  Path to library directory | Relative (to config) or absolute path | D:\Aspose\asposecpplib


**Default value**: ../../../../asposecpplib

#### cmake_temaplates or makefile_templates ####

Path to the directory with CMakeLists.txt file to be used as a template.

| Allowed value | Meaning | Example
---| ---| ---|
| Path to the directory | Path or name of the directory containing CMakeLists.txt template | MyTemplates


**Default value**: CMake

#### generatedlist_template ####

Path to a file which will be used as a template for outputting list of all generated sources (header and cpp files)

| Allowed value | Meaning | Example
---| ---| ---|
|Path to the file|Path or name of a template file|cmake/GeneratedList.cmake

**Default value**: "" (empty string)

Let's suppose you have the following GeneratedList.cmake file:

{{< highlight xml >}}
set(generatedhpp
%%HEADERS%%
)

set(generatedcpp
%%SOURCES%%
)

{{< /highlight >}}



The porter will create file GeneratedList.cmake next to CMakeLists.txt with the following content:

{{< highlight xml >}}
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
{{< /highlight >}}


Now you can include this file from CMakeLists.txt and use generatedcpp and generatedhpp variables instead of file(GLOB) cmake command.

#### include_templates ####

Path to the directory with shared_api_defs.h template file used to generate shared library API.

| Allowed value | Meaning | Example
---| ---| ---|
|  Path to the directory |Path to the directory containing shared_api_defs.h template | MyTemplates


**Default value**: Include

#### source_templates ###

Path to the directory with embedded_resources.cpp template to support Assembly class and C# project's resources access.

| Allowed value | Meaning | Example
---| ---| ---|
| Path to the directory | Path to the directory containing embedded_resources.cpp template | MyTemplates


**Default value**: Source

#### add_assembly_details ####

Indicates for which returned assembly current project will be used. I.e for executing_assembly Assembly.GetExecutingAssembly() will be used to access assembly name resources etc.

| Allowed value | Meaning | Example
---| ---| ---|
| executing_assembly | Use assembly executing at given moment |
| entry_assembly | Use assembly used as an entry point |  
| calling_assembly | Use assembly calling into current one |




**Default value**: <None>

#### additional_defines ####

Additional defines for either C# code (used during code parsing) or C++ project (passed to cmake).

| Allowed value | Meaning | Example
---| ---| ---|
| List of defines to use | List of the defiles. Separators are space (' ') and semicolon (';') | ~_~_cplusplus;UNIT_TEST MY_DEFINE


**Default value**: ~_~_cplusplus

| Additional attribute |  Meaning |  Allowed values | Mandatory |  Default value  
---| ---| ---| ---| ---|
| cmakeonly | Whether the define goes only to C++ project and not to C# project | true false | No | false
| csonly | whether the define goes to only to C# project and not to C++ project | true false | No | false


#### exclude_conditional_symbols ####

Exclude defines from being passed from C# project to cmake. Normally, porting applications passes all definitions mentioned in project file to cmake.

| Allowed value | Meaning | Example
---| ---| ---|
| List of defines | Defines in C# project that won't be passed to C++ project. Separators are space (' ') and semicolon (';') | MY_DEFINE MY_DEFINE_2;MY_DEFINE_3


**Default value**: <None>

#### additional_includes ####

Additional includes to pass to ported project via cmake.

| Allowed value | Meaning | Example
---| ---| ---|
| List of directories | List of additional include directories passed to cmake. Separators are space (' ') and semicolon (';') | C:\Cpp\my_lib;C:\Cpp\my_lib_2 C:\Cpp\third_party_lib


**Default value**: <Not defined>

| Additional attribute |  Meaning |  Allowed values | Mandatory |  Default value  
---| ---| ---| ---| ---|
|  local| If this path is mentioned in porter-generated include, whether to cut generated include to relative path | true false |  No | false


#### custom_gtest_main ####

Path to the custom file with gtest main() function to use instead of the default one.

| Allowed value | Meaning | Example
---| ---| ---|
| Path to the file | Path to the file containing main() function to call into gtest. | custom_gtest_main.cpp

**Default value**: gtest_main.cc

#### cpp_files_to ####

Directory to copy cpp files contained in the current project too.

| Allowed value | Meaning | Example
---| ---| ---|
| Directory name | Directory name inside output project folder | cpp_files


**Default value**: source

#### interface_as_public ####

Unconditionally move all interfaces to public headers, including non-public ones.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Move all interfaces to public headers |
| false | Put public interfaces to public headers, put private interfaces to private headers |



**Default value**: false

#### internal_as_public ####

Whether to translate internal members and types as public. Useful when preparing the library to be linked with tests project.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Translate internal members as public ones. |
| false | Translate internal members as private ones, but generate 'friend' declaration for inter-class access if required. |



**Default value**: false

#### do_not_hardcode_aspose_cpp_path ####

Disables writing exact path to asposecpplib at CMakeLists.txt. Useful if converted project is compiled from directory different from the one it was ported into.

| Allowed value | Meaning | Example
---| ---| ---|
| true | Do not put library path to CMakeLists.txt. |
| false | Put library path to CMakeLists.txt. |



**Default value**: false

#### tools_version ####

Specific the version of tool that should be used on porting stage. Useful if project converted on machine with higher version of .NET Framework.

| Allowed value | Meaning | Example
---| ---| ---|
| Tools version | Tools version recognized by MSBuild. |4.0 14.0


**Default value**: a version of tool specified in the project file or any available one if the project file doesn't specify any.

#### target_framework_version ####

The specific version of .NET Framework to use when parsing C# project.

| Allowed value | Meaning | Example
---| ---| ---|
| .NET framework version | Framework available at "%ProgramFiles(x86)%\Reference Assemblies\Microsoft\Framework\.NETFramework\" | v4.5 v4.5.1 v4.5.2 v4.6 v4.6.1 v4.6.2 v4.7 v4.7.1


**Default value**: Version specified in the project file.

#### make_library ####

An alternative way to specify output library type in what relates to shared API macros.

| Allowed value | Meaning | Example
---| ---| ---|
|shared|Create shared library|Same as make_shared_lib#true
|static|Create static library|Same as make_shared_lib#false
|api|Create a library with API export macros. Use this if you are building e. g. the shared library which consists of several static ones and need generate shared library exports when translating into static libraries.|

**Default value**: static

| Additional attribute |  Meaning |  Allowed values | Mandatory |  Default value  
---| ---| ---| ---| ---|
|hide_local_symbols|Whether to avoid exporting private symbols.|true false|false|false
|export_per_member|If true, export each member separately. If false, export whole classes.|true false|false|true
|export_internals|If true, export internal members.|true false|false|false
|shared_id|Export macro prefix|Identifier|false|Generated based on assembly name

#### generate_includes_subdirectory ####

Creates a subdirectory under 'include' directory to avoid header name clashes when using several ported projects from a single project.

| Allowed value | Meaning | Example
---| ---| ---|
|false|Do not create subdirectory|include/MyClass.h is a file for MyClass.
|true|Create subdirectory named after the C# project unless the name is specified explicitly.|include/MyProject/MyClass.h is a file for MyClass from C# 'MyProject' project.

**Default value**: false

| Additional attribute |  Meaning |  Allowed values | Mandatory |  Default value  
---| ---| ---| ---| ---|
|directory|Explicit name of the subdirectory under 'include' folder.|String value|false|C# project name

## make\_cpp\_file\_name\_uniq

Controls porter behavior in whether file names should be unicalized by extending with trailing underscores.

| **Allowed value** | **Meaning** |
| --- | --- |
| true | All file names are unicalized. |
| false | File names may repeat. |

**Default value:** true
**Since version:** 20.8

# Code readability

These options improve generated code's readability. However, the code generated now doesn't handle some corner cases properly or in the same way C# code does, so using these options on big codebases is error-prone. Instead, use them to port e. g. code samples for your projects being ported, to make them easy to read.

## foreach\_as\_range\_based\_for\_loop

Translate C# foreach loops as C++ [range-based for loops](https://en.cppreference.com/w/cpp/language/range-for)

| foreach (HeaderFooter hf in doc.GetChildNodes(NodeType.HeaderFooter, true)){    // ...} |
| --- |

| **Allowed value** | **Meaning** | **Example** |
| --- | --- | --- |
| **false** | Translate foreach loop as while loop |

| auto hf\_enumerator = doc-\&gt;GetChildNodes(NodeType::HeaderFooter, true)-\&gt;GetEnumerator();SharedPtr\&lt;HeaderFooter\&gt; hf;while (hf\_enumerator-\&gt;MoveNext() &amp;&amp; (hf = DynamicCast\&lt;HeaderFooter\&gt;(hf\_enumerator-\&gt;get\_Current()), true)){    // ...} |
| --- |

 |
| **true** | Translate foreach loop as range-based for loop |

| for (auto hf : IterateOver\&lt;HeaderFooter\&gt;(doc-\&gt;GetChildNodes(NodeType::HeaderFooter, true)) ){    // ...} |
| --- |

 |

**Default value** : false

## simplify\_using\_statements

Makes porter generate more compact code for using statements that relies on used object destructors rather then on correct Dispose calls.

| **Allowed value** | **Meaning** | **Example** |
| --- | --- | --- |
| true | Do not generate compilcated code to call into Dispose(). | {
     System::SharedPtr <Rs>; \_\_using\_resource\_0 = System::MakeObject <Rs> ();
     System::Console::WriteLine("Statement");
 } |
| false | Generate correct Dispose calls anyway. | {
     System::SharedPtr <Rs> \_\_using\_resource\_0 = System::MakeObject <Rs>

     // Clearing resources under using statement
     System::Details::DisposeGuard <1> \_\_dispose\_guard\_1({ \_\_using\_resource\_0});
     // ------------------------------------------

     try
     {
         System::Console::WriteLine("Statement");
     }
     catch(...)
     {
         \_\_dispose\_guard\_1.SetCurrentException(std::current\_exception());
     }
 } |

**Default value:** false
**Since version:** 20.8

## force\_auto\_in\_variable\_declaration

Makes porter generate auto types for local variables instead of full type name so that code is more compact.

| **Allowed value** | **Meaning** | **Example** |
| --- | --- | --- |
| true | Generate &#39;auto&#39; type names. | auto rs = System::MakeObject <Rs> ()) |
| false | Generate full type names. | System::SharedPtr <Rs>; rs = System::MakeObject <Rs> () |

**Default value:** false
**Since version:** 20.8

## prefer\_short\_type\_names

Makes porter prefer short type names where possible instead of fully qualified names in some contexts.

| **Allowed value** | **Meaning** | **Example** |
| --- | --- | --- |
| true | Use short names. | System::StaticCast <A>;o) |
| false | Use fully qualified names. | System::StaticCast;Full::Namespace::Path:: <A>(o) |

**Default value:** false
**Since version:** 20.9

## use\_stream\_based\_io

Replaces System::Console calls with cout invocations.

| **Allowed value** | **Meaning** | **Example** |
| --- | --- | --- |
| true | Switch to cout usage. | std::cout; Hello; std::endl; |
| false | Use fully qualified names. | System::Console::WriteLn("Hello"); |

**Default value:** false
**Since version:** 20.10

### Legacy options ###

Options no longer supported but still recognized (and ignored) by porting application for compatibility reasons are:

* alternative_base
* boost_ver
* additional_libdirs
* singleton_mode
* auto_weak_ptr_reference
* gtest_path
* insert_using_statement_guard
* using_statement_as_lambda
*  using_statement_enhanced 
