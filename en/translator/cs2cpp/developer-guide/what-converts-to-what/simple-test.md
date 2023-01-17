---
date: "2023-01-16"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "SimpleTest"
linktitle: "SimpleTest"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2023-01-16"
weight: "1"
---

This example demonstrates how simple NUnit test is translated to C++. Googletest C++ library is used to translate NUnit tests to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using NUnit.Framework;

namespace NUnitTestsPorting
{
    [TestFixture]
    public class SimpleTest
    {
        [Test]
        public void T1()
        {
        }
    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <gtest/gtest.h>

namespace NUnitTestsPorting {

class SimpleTest : public System::Object
{
    typedef SimpleTest ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void T1();
    
};

} // namespace NUnitTestsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "SimpleTest.h"

namespace NUnitTestsPorting {

RTTI_INFO_IMPL_HASH(1656418294u, ::NUnitTestsPorting::SimpleTest, ThisTypeBaseTypesInfo);

namespace gtest_test
{

class SimpleTest : public ::testing::Test
{
protected:
    static System::SharedPtr<::NUnitTestsPorting::SimpleTest> s_instance;
    
    static void SetUpTestCase()
    {
        s_instance = System::MakeObject<::NUnitTestsPorting::SimpleTest>();
    };
    
    static void TearDownTestCase()
    {
        s_instance = nullptr;
    };
    
};

System::SharedPtr<::NUnitTestsPorting::SimpleTest> SimpleTest::s_instance;

} // namespace gtest_test

void SimpleTest::T1()
{
}

namespace gtest_test
{

TEST_F(SimpleTest, T1)
{
    s_instance->T1();
}

} // namespace gtest_test

} // namespace NUnitTestsPorting

{{< /highlight >}}
