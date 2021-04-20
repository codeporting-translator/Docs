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

Some attributes are allowed to be specified in configuration files to save editing the source code.

Any 'attribute' definition can now be assigned 'error_if_unused' XML attribute so the porter will produce an error message if the attribute is unused when porting a project, as follows:

{{< highlight xml >}}
<attribute error_if_unused="true" name="CppConstMethod" class="PorterAttributes.UnusedConfigAttributes" method="System.Void Foo()"/>
{{< /highlight >}}

The below lists the attributes that are available from configuration files.

## CppSkipEntity ##

### C# source code ###

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

### porter.config ###

{{< highlight xml >}}
<attribute name="CppSkipEntity" interface="System.Collections.Generic.IEnumerable" method="System.Collections.IEnumerator GetEnumerator()"/>
<attribute name="CppSkipEntity" set="public System.Void someProperty(System.Int32)"/>
<attribute name="CppSkipEntity" get="public System.Int32 someProperty()"/>
{{< /highlight >}}

In this section all things - interfaces, classes and so on, set by name with selected method, will be ignored while converting.

Get and set methods of properties also included.

Method should be in full form: access modification - public, protected, private or default. Default assumed that there is nothing to write. Next static modification, if such present, full return type, name of method, parameters type in full form (System.Int32, System.String, System.Void and so on), if method takes something, and constant modification if such present.

Please note, if method(s) of property should be skipped, value of set/get should contain property name of such method.

### C++ source code ###

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

## CppSkipTest ##

### C# source code ###

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

## CppSkipTest ##

### porter.config ###

{{< highlight xml >}}
<attribute name="CppSkipTest" interface="SkipTestAttribute" method="public void SkippedTest()"/>
{{< /highlight >}}

### C++ source code ###

{{< highlight cpp >}}
//...
TEST_F(SkipTestAttribute, SkippedTest)
{
    GTEST_SKIP();
    s_instance->SkippedTest();
}
//...
{{< /highlight >}}

## CppConstMethod ##

### C# source code ###

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

### porter.config ###

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

### C++ source code ###

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

## CppPortConstStringAsWChar ##

Force to transformate const string as const wchar_t* on C++ side.

Sample:

### porter.config ###

{{< highlight xml >}}
<attribute name="CppPortConstStringAsWChar" field="SampleCsProject.Attributes.PortConstStringAsWCharTest.WCharValueByConfig" condition="field"/>
{{< /highlight >}}

### C# source code ###

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

## CppRenameEntity ##

Allow to rename delegates from porter.config

Sample using

### porter.config ###

{{< highlight xml >}}
<attribute name="CppRenameEntity" parameter="Func1" namespace="SampleCsProject.Attributes.UsingViaConfig" delegate="public TResult Func&lt;out TResult&gt;()" condition="delegate"/>
<attribute name="CppRenameEntity" parameter="Func2" namespace="SampleCsProject.Attributes.UsingViaConfig" delegate="public TResult Func&lt;in T, out TResult&gt;(T arg)" condition="delegate"/>
<attribute name="CppRenameEntity" parameter="Func3" namespace="SampleCsProject.Attributes.UsingViaConfig" delegate="public TResult Func&lt;in T1, in T2, out TResult&gt;(T1 arg1, T2 arg2)" condition="delegate"/>
{{< /highlight >}}

### C# source code ###

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