---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Attributes in Configuration file"
linktitle: "Attributes in Configuration file"
description: "Attributes Specified Configuration file"
menu:
  docs:
    parent: "CodePorting Native Cs2Cpp Configuration File"
    weight: "2"
lastmod: "2019-05-28"
weight: "2"
---

It is possible to define some code attributes to porter configuration file. There are two benefits from doing so:

1. The C# source code remains cleaner, so .Net developers don't get confused by unrelated attributes.
1. The single line in the configuration file can apply attribute to multiple items at once, so you have less code.

The code attribute is defined by the 'attribute' XML node with some XML attributes. The example syntax is below.

{{< highlight xml >}}
<attribute error_if_unused="true" name="CppConstMethod" class="PorterAttributes.UnusedConfigAttributes" method="System.Void Foo()"/>
{{< /highlight >}}

There allowed XML attributes fall into several categories:

| Category | Meaning | Example XML attributes
---| ---| ---|
| Attribute name | Defines which attribute to apply to specific items | name
| Attribute condition | Defines what items to apply the attribute to | condition, class, interface, struct, method, property, indexer, get, set, operator, field, paremetername
| Attribute parameters | Defines the attribute parameters | argument, argument0, argument1, ..., argumentN, parameterkind
| Attribute additional behavior | Defines how porter should behave when workin on this attribute definition | error_if_unused

## Defining C# attrbitues in configuration file ##

The below guide shows how to define code attributes in configuration file. There are several steps to do so:

1. Define '&lt;attribute&gt;' XML tag somewhere in the configuration file. Make sure this file is used with all projects you port that use the item(s) you need to attribute.
1. Specify the code attribute you want to apply to items in your code by using 'name' XML attribute.
1. Specify the arguments to this code attribute (if required). To do so, use specific XML attributes.
1. Define the context (item or several items) to which the code attribute gets applied by specifying proper XML attributes.
1. Define additional behavior for the code attribute (if required).

The below sections explain these points in details.

### Attribute tag in configuration file ###

As shown above, the syntax for adding the code attribute in the configuration files is as follows:

{{< highlight xml >}}
<attribute error_if_unused="true" name="CppConstMethod" class="PorterAttributes.UnusedConfigAttributes" method="System.Void Foo()"/>
{{< /highlight >}}

Adding the code attribute to the configuration file makes porter behave as if this attribute was present in the code where specified. However, it will only work if the respectful configuration file is used. So, if there's more than one project being ported that makes use of the same item (class, struct, method, etc.), it is important to preserve consistensy of the configuration files used to translate each of the projects and make sure all of them include definition for this attribute. One of the easiest ways to do so is to define the attribute in the configuration file which is included from configuration files used to port all dependent projects. See [Configuration file nodes](/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-nodes/) manual for more information.

'name' XML attribute should have a value of C# attribute name (without namespace) one wants to use. So, to use System.Obsolete attribute, one should simply use 'Obsolete' name. To use CsToCppPorter.CppConstMethod attribute, one should specify 'CppConstMethod' name.

### Attribute context ###

It is important to specify what item in the source code the attribute must be applied to. This is being done by XML attributes from the condition group.

There are different ways to specify e. g. an attribute that applies to the class, an attribute that applies to the class method and an attribute that applies to the class method parameter. We refer to these ways as to conditions, each condition defining where to apply its attribute.

Any condition may be used with any C# attribute, as long as it makes sense. For example, it is possible to apply CppConstMethod attribute to a method, but not to a class or a field. In other means, the conditions are completely independent from C# attributes: one may use different types of conditions with same attribute or use same condition type with different attributes.

To tell the porter which condition type to use, one must specify the 'condition' attribute of the '&lt;attribute&gt;' XML tag. If this attribute is not specified, 'method_in_class_or_baseclass' condition type is used.

The below sections summarize what conditions are available and how to use them.

#### method_in_class_or_baseclass condition ####

This condition applies its attribute to:

1. Given method, getter, setter or property in the class, interface or structure;
1. Method, getter, setter or property that overrides or implements the specified member.

The following XML attributes are mandatory for this condition:

1. Type attribute, which contains the fully qualified name of the type:
    1. 'class' for a class,
    1. 'interface' for an interface, or
    1. 'struct' for a structure.
1. Member attribute, which contains the signature of the member: access modifier (optional), 'static' modifier (optional), return type, name and argument type list:
    1. 'method' for a method,
    1. 'get' for a getter, with name being property name or 'Item' for an indexer, and argument list being empty for properties or containing indexer argument list,
    1. 'set' for a setter, with name being property name or 'Item' for an indexer, and argument list being empty for properties or containing indexer argument list,
    1. 'property' for a property, with name being property name and argument list being empty,
    1. 'operator' for an operator.

In argument list, it is possible to use '?' as a substitution which means 'one parameter of any type', which is useful for generic parameters. It is also possible to use '*' as a substituion in argument list ('any number of parameters of any types'), return type ('any type') or method name ('any name'). Examples are below.

{{< highlight xml >}}
<!-- Method of specific class (and overrides) -->
<attribute name="CppConstMethod" class="System.Object" method="public System.Int32 GetHashCode()"/>
<!-- Method of any class (and overrides) -->
<attribute name="CppRenameEntity" method="protected bool Equals(?)" class="*"/>
<!-- Any method of the class (and overrides) -->
<attribute name="CppRenameEntity" method="public static * *(*)" class="System.Linq.Enumerable"/>
<!-- Property getter -->
<attribute name="CppConstMethod" class="System.Exception" get="System.String Message()"/>
<attribute name="CppSkipEntity" interface="System.Collections.IEnumerator" property="System.Object Current()"/>
<!-- Indexer getter -->
<attribute name="CppConstMethod" get="* Item(?)" interface="System.Collections.Generic.IDictionary"/>
<!-- Property setter -->
<attribute name="CppInline" class="MyClass" set="void Message(string)"/>
<!-- Indexer setter -->
<attribute name="CppInline" set="void Item(*)" interface="System.Collections.Generic.IDictionary"/>
<!-- Operator -->
<attribute name="CppArgumentKind" operator="public static bool op_Equality(*)" struct="*" parametername="*" parameterkind="ConstReference" condition="parameter"/>
{{< /highlight >}}

#### field condition ####

This condition applies its attribute to the field. The 'field' XML attribute must be specified containing fully qualified name of the field:

{{< highlight xml >}}
<attribute name="CppWeakPtr" condition="field" field="Namespace.ClassName.FieldName"/>
{{< /highlight >}}

#### delegate condition ####

This condition applies its attribute to the delegate. The 'delegate' XML attribute must contain full signature of the delegate:

{{< highlight xml >}}
<attribute name="CppSkipEntity" condition="delegate" delegate="public void MyDelegate&lt;T, U&gt;(T a, U b, int c)"/>
{{< /highlight >}}

#### constructor condition ####

This condition applies its attribute to the constructor. The 'constructor' XML attribute must contain full signature of the constructor:

{{< highlight xml >}}
<attribute name="CppCTORSelfReference" class="Namespace.Class" condition="constructor" constructor="public Class(Parameter.Type)"/>
{{< /highlight >}}

#### type condition ####

This condition applies its attribute to the type. Mandatory type attribute ('class', 'interface' or 'struct') must contain the fully qualified name of the type to apply attribute to:

{{< highlight xml >}}
<attribute name="CppDeclareFriendClass" argument="Namespace.FriendClassName" class="Namespace.ClassName" condition="type"/>
{{< /highlight >}}

#### basetype condition ####

This condition applies its attribute to the specified type and to all types that inherit it. Mandatory type attribute ('class', 'interface' or 'struct') must contain the fully qualified name of the type to apply attribute to:

{{< highlight xml >}}
<attribute name="CppDeclareFriendClass" argument="Namespace.FriendClassName" class="Namespace.ClassName" condition="basetype"/>
{{< /highlight >}}

#### parameter condition ####

This condition applies its attribute to the parameter of specified method or constructor of specified type or its subtype that overrides or implements the method. It must contain mandatory type and method XML attributes (see method_in_class_or_baseclass condition description for details) and mandatory 'parametername' XML attribute with the name of parameter to apply attribute to (or '*' to apply it to all parameters).

{{< highlight xml >}}
<attribute name="CppArgumentKind" method="* int Compare(?, ?)" interface="System.Collections.Generic.IComparer" parametername="*" parameterkind="ConstReference" condition="parameter"/>
<attribute name="CppArgumentKind" method="void Insert(?, ?)" interface="System.Collections.Generic.IList" parametername="item" parameterkind="ConstReference" condition="parameter"/>
<attribute name="CppLambdaShouldCaptureByReference" method="public * *(*)" class="System.Collections.Generic.List" parametername="match" condition="parameter" />
{{< /highlight >}}

### Attribute arguments ###

Some attributes may have arguments (mandatory or optional). For example, CppArgumentKind attribute requires the kind of the argument to be specified. CppRenameEntity attribute may take a new name for the enrity.

To specify a single argument, one may use 'argument' XML attribute. To specify several ordered arguments, one may use 'argument0', 'argument1', 'argument2' attributes and so on:

{{< highlight xml >}}
<attribute name="CppConstMethod" get="bool CanRead()"  class="System.IO.Stream" argument="true"/>
<attribute name="CppIOStreamWrapper" method="void MyMethod(*)"  class="MyClass" argument0="CharType" argument0="TraitsType"/>
{{< /highlight >}}

Some C# attributes allow using named arguments as well. Currently, only 'parameterkind' argument for CppArgumentKind attribute is allowed.

### Special instructions ###

There can be special instructions for the porter on how to handle specific C# attribute specified in config file. These instructions are also given in form of XML attributes to '&lt;attribute&gt;' tag.

#### error_if_unused instruction ####

This attribute, if set to true, makes the porter report an error if the attribute was never applied to any item in the code. This may be useful to track situations when the code was changed, but the attributes for it were not. By default, this behavior is turned off.

{{< highlight xml >}}
<attribute error_if_unused="true" name="CppConstMethod" class="PorterAttributes.UnusedConfigAttributes" method="System.Void Foo()"/>
{{< /highlight >}}

In this example, if 'PorterAttributes.UnusedConfigAttributes.Foo()' method or its overrides was not met during code translation, the porter will raise an error.

## Examples ##

The below examples show how specific attributes are applied to the ported code from configuration files.

### CppSkipEntity ###

#### C# source code ####

{{< highlight cs >}}
public class StringEnumerable : IEnumerable<string>
{
    public IEnumerator<string> GetEnumerator()
    {
        //...
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}

class GetSetClass
{
    private int i;

    public int someProperty
    {
        get
        {
            return i;
        }
        set
        {
            i = value;
        }
    }
}
{{< /highlight >}}

#### porter.config ####

{{< highlight xml >}}
<attribute name="CppSkipEntity" interface="System.Collections.Generic.IEnumerable" method="System.Collections.IEnumerator GetEnumerator()"/>
<attribute name="CppSkipEntity" set="public System.Void someProperty(System.Int32)"/>
<attribute name="CppSkipEntity" get="public System.Int32 someProperty()"/>
{{< /highlight >}}

In this section all things - interfaces, classes and so on, set by name with selected method, will be ignored while converting.

Get and set methods of properties also included.

Method should be in full form: access modification - public, protected, private or default. Default assumed that there is nothing to write. Next static modification, if such present, full return type, name of method, parameters type in full form (System.Int32, System.String, System.Void and so on), if method takes something, and constant modification if such present.

Please note, if method(s) of property should be skipped, value of set/get should contain property name of such method.

#### C++ source code ####

{{< highlight cpp >}}
class StringEnumerable : public System::Collections::Generic::IEnumerable<System::String>
{
public:

    System::SharedPtr<System::Collections::Generic::IEnumerator<System::String>> GetEnumerator()
    {
        //...
    }
};

class GetSetClass : public System::Object
{
private:
    int32_t i;
};
{{< /highlight >}}

### CppSkipTest ###

#### C# source code ####

{{< highlight cs >}}
[TestFixture]
public class SkipTestAttribute
{
    [Test]
    [CsToCppPorter.CppSkipTest]
    public void SkippedTest()
    {
    }
}
{{< /highlight >}}

### CppSkipTest ###

#### porter.config ####

{{< highlight xml >}}
<attribute name="CppSkipTest" interface="SkipTestAttribute" method="public void SkippedTest()"/>
{{< /highlight >}}

#### C++ source code ####

{{< highlight cpp >}}
//...
TEST_F(SkipTestAttribute, SkippedTest)
{
    GTEST_SKIP();
    s_instance->SkippedTest();
}
//...
{{< /highlight >}}

### CppConstMethod ###

#### C# source code ####

{{< highlight cs >}}
public class IntEnumerator : IEnumerator<int>
{
    public int Current
    {
        get
        {
            return 0;
        }
    }

    object System.Collections.IEnumerator.Current
    {
        get
        {
            return Current;
        }
    }

    public void Dispose()
    {
    }

    public bool MoveNext()
    {
        return false;
    }

    public void Reset()
    {
    }
}

public class IntComparer : IComparer<int>
{
    public int Compare(int a, int b)
    {
        return (a < b) ? -1 : (a == b) ? 0 : 1;
    }
}

public class StringEnumerator : IEnumerator<string>
{
    public string Current
    {
        get
        {
            return "StringEnumerator";
        }
    }

    object System.Collections.IEnumerator.Current
    {
        get
        {
            return Current;
        }
    }

    public void Dispose()
    {
    }

    public bool MoveNext()
    {
        return false;
    }

    public void Reset()
    {
    }
}

public class StringOrdinalComparer : IComparer<string>
{
    public int Compare(string a, string b)
    {
        return string.Compare(a, b);
    }
}
{{< /highlight >}}

#### porter.config ####

{{< highlight xml >}}
<attribute name="CppSkipEntity" interface="System.Collections.Generic.IEnumerator" get="System.Object Current()"/> <!-- mandatory for success compile on C+\+ side, see explanation of this attribute on this page -->
<attribute name="CppConstMethod" interface="System.Collections.Generic.IEnumerator" get="System.Int32 Current()"/>
<attribute name="CppConstMethod" interface="System.Collections.Generic.IEnumerator" get="System.String Current()"/>
<attribute name="CppConstMethod" interface="System.Collections.Generic.IComparer" method="System.Int32 Compare(System.Int32, System.Int32)"/>
<attribute name="CppConstMethod" interface="System.Collections.Generic.IComparer" method="System.Int32 Compare(System.String, System.String)"/>
<!-- or -->
<attribute name="CppConstMethod" interface="System.Collections.Generic.IEnumerator" get="0 Current()"/>
<attribute name="CppConstMethod" interface="System.Collections.Generic.IComparer" method="System.Int32 Compare(0, 0)"/>
{{< /highlight >}}

In this section all things - interfaces, classes and so on, set by name with selected method, will be with const modification after converting process end.

Get and set methods of properties also included.

Method should be in full form: access modification - public, protected, private or default. Default assumed that there is nothing to write. Next static modification, if such present, full return type, name of method, parameters type in full form (System.Int32, System.String, System.Void and so on), if method takes something, and constant modification if such present.

For template argument of things located in .NET Framework it also can be set 0 as first type argument record.

Template argument of own things "support" only on basic class - that menace you do not get code with const in children method.

Please note, if method(s) of property should be skipped, value of set/get should contain property name of such method.

#### C++ source code ####

{{< highlight cpp >}}
#include <system/string.h>
#include <cstdint>

int32_t IntEnumerator::get_Current() const
{
    return 0;
}

bool IntEnumerator::MoveNext()
{
    return false;
}

void IntEnumerator::Reset() { }

int32_t IntComparer::Compare(int32_t a, int32_t b) const
{
    return (a < b) ? -1 : (a == b) ? 0 : 1;
}


System::String StringEnumerator::get_Current() const
{
    return L"StringEnumerator";
}

bool StringEnumerator::MoveNext()
{
    return false;
}

void StringEnumerator::Reset() { }

int32_t StringOrdinalComparer::Compare(System::String a, System::String b) const
{
    return System::String::Compare(a, b);
}
{{< /highlight >}}

### CppPortConstStringAsWChar ###

Force to transformate const string as const wchar_t* on C++ side.

Sample:

#### porter.config ####

{{< highlight xml >}}
<attribute name="CppPortConstStringAsWChar" field="SampleCsProject.Attributes.PortConstStringAsWCharTest.WCharValueByConfig" condition="field"/>
{{< /highlight >}}

#### C# source code ####

{{< highlight cs >}}
using System;
using NUnit.Framework;

namespace SampleCsProject.Attributes
{
    [TestFixture]
    class PortConstStringAsWCharTest
    {
        const string WCharValueByConfig = "def";
    }
}
{{< /highlight >}}

Please note, that all strings located after WCharValueByConfig by comma, will also be wchar_t*, as this attribute set on field, not on variable in field directly. Comparing to attribute directly in C# code, in this case response on variable name in config will be on person who created such.

### CppRenameEntity ###

Allow to rename delegates from porter.config

Sample using

#### porter.config ####

{{< highlight xml >}}
<attribute name="CppRenameEntity" parameter="Func1" namespace="SampleCsProject.Attributes.UsingViaConfig" delegate="public TResult Func&lt;out TResult&gt;()" condition="delegate"/>
<attribute name="CppRenameEntity" parameter="Func2" namespace="SampleCsProject.Attributes.UsingViaConfig" delegate="public TResult Func&lt;in T, out TResult&gt;(T arg)" condition="delegate"/>
<attribute name="CppRenameEntity" parameter="Func3" namespace="SampleCsProject.Attributes.UsingViaConfig" delegate="public TResult Func&lt;in T1, in T2, out TResult&gt;(T1 arg1, T2 arg2)" condition="delegate"/>
{{< /highlight >}}

#### C# source code ####

{{< highlight cs >}}
using System;
using NUnit.Framework;

namespace SampleCsProject.Attributes
{
    namespace UsingViaConfig
    {
        public delegate TResult Func<out TResult>();

        public delegate TResult Func<in T, out TResult>(T arg);

        public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
    }
}
{{< /highlight >}}

Please note, that version that available from porter.config allow rename only delegates. At least implemented and tested only for such purpose.

Use same named attribute in C# code for rename other supported things.