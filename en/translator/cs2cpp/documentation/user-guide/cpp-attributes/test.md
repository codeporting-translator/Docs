---
order: "1"
navTitle: "Unit tests"
---

# Test frameworks attributes #

This section describes attributes belonging to different test frameworks that are recognized by the translator and translated into gTest tests on the C++ side.

[TOC]

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
