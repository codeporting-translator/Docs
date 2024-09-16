---
date: "2024-09-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "TestWithSetupMethods"
linktitle: "TestWithSetupMethods"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2024-09-10"
weight: "1"
---

This example demonstrates how NUnit test fixture with SetUp method is translated to C++. Googletest C++ library is used to translate NUnit tests to C++. SetUp, TearDown, TestFixtureSetUp, and TestFixtureTearDown methods are supported and translated to corresponding methods of googletest.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using NUnit.Framework;

namespace NUnitTestsPorting
{
    [TestFixture]
    public class TestWithSetupMethods
    {
        [SetUp]
        public void Setup()
        {
            mValue += 1;
        }

        [TearDown]
        public void TearDown()
        {
            mValue -= 1;
        }

        [TestFixtureSetUp]
        public void GlobalSetup()
        {
            mValue += 10;
        }

        [TestFixtureTearDown]
        public void GlobalTearDown()
        {
            mValue -= 10;
        }

        [Test]
        public void TestMethod()
        {
        }

        private int mValue = 0;
    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <gtest/gtest.h>
#include <cstdint>

namespace NUnitTestsPorting {

class TestWithSetupMethods : public System::Object
{
    typedef TestWithSetupMethods ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void Setup();
    void TearDown();
    void GlobalSetup();
    void GlobalTearDown();
    void TestMethod();
    
    TestWithSetupMethods();
    
protected:

    int32_t mValue;
    
};

} // namespace NUnitTestsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "TestWithSetupMethods.h"

namespace NUnitTestsPorting {

RTTI_INFO_IMPL_HASH(1963724351u, ::NUnitTestsPorting::TestWithSetupMethods, ThisTypeBaseTypesInfo);

void TestWithSetupMethods::Setup()
{
    mValue += 1;
}

void TestWithSetupMethods::TearDown()
{
    mValue -= 1;
}

void TestWithSetupMethods::GlobalSetup()
{
    mValue += 10;
}

void TestWithSetupMethods::GlobalTearDown()
{
    mValue -= 10;
}

TestWithSetupMethods::TestWithSetupMethods() : mValue(0)
{
}


namespace gtest_test
{

class TestWithSetupMethods : public ::testing::Test
{
protected:
    static System::SharedPtr<::NUnitTestsPorting::TestWithSetupMethods> s_instance;
    
    void SetUp() override
    {
        s_instance->Setup();
    };
    
    void TearDown() override
    {
        s_instance->TearDown();
    };
    
    static void SetUpTestCase()
    {
        s_instance = System::MakeObject<::NUnitTestsPorting::TestWithSetupMethods>();
        s_instance->GlobalSetup();
    };
    
    static void TearDownTestCase()
    {
        s_instance->GlobalTearDown();
        s_instance = nullptr;
    };
    
};

System::SharedPtr<::NUnitTestsPorting::TestWithSetupMethods> TestWithSetupMethods::s_instance;

} // namespace gtest_test

void TestWithSetupMethods::TestMethod()
{
}

namespace gtest_test
{

TEST_F(TestWithSetupMethods, TestMethod)
{
    s_instance->TestMethod();
}

} // namespace gtest_test

} // namespace NUnitTestsPorting

{{< /highlight >}}
