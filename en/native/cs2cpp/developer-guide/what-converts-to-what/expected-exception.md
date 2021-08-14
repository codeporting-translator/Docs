---
date: "2021-08-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ExpectedException"
linktitle: "ExpectedException"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-08-09"
weight: "1"
---

This example demonstrates how NUnit test with expected exception attribute applied is ported to C++. Googletest C++ library is used to translate NUnit tests to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;
using NUnit.Framework;

namespace NUnitTestsPorting
{
    [TestFixture]
    public class ExpectedException
    {
        [Test]
        [ExpectedException]
        public void TestMethod1()
        {
        }

        [Test]
        [ExpectedException(typeof(InvalidOperationException))]
        public void TestMethod2()
        {
        }

        [Test]
        [ExpectedException("System.InvalidOperationException")]
        public void TestMethod3()
        {
        }

        [Test]
        [ExpectedException(typeof(InvalidOperationException), ExpectedMessage = "expected message")]
        public void TestMethod4()
        {
        }

        [Test]
        [ExpectedException(typeof(InvalidOperationException), ExpectedMessage = "expected message", MatchType = MessageMatch.Contains)]
        public void TestMethod5()
        {
        }

        [Test]
        [ExpectedException(typeof(InvalidOperationException), UserMessage = "user message")]
        public void TestMethod6()
        {
        }

        [Test]
        [ExpectedException(typeof(InvalidOperationException), Handler = "HandleException")]
        public void TestMethod7()
        {
        }

        public void HandleException(Exception e)
        {
        }
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <system/exceptions.h>
#include <gtest/gtest.h>

namespace NUnitTestsPorting {

class ExpectedException : public System::Object
{
    typedef ExpectedException ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void TestMethod1();
    void TestMethod2();
    void TestMethod3();
    void TestMethod4();
    void TestMethod5();
    void TestMethod6();
    void TestMethod7();
    void HandleException(System::Exception e);
    
};

} // namespace NUnitTestsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ExpectedException.h"

#include <system/exceptions.h>

namespace NUnitTestsPorting {

RTTI_INFO_IMPL_HASH(1546777381u, ::NUnitTestsPorting::ExpectedException, ThisTypeBaseTypesInfo);

void ExpectedException::HandleException(System::Exception e)
{
}


namespace gtest_test
{

class ExpectedException : public ::testing::Test
{
protected:
    static System::SharedPtr<::NUnitTestsPorting::ExpectedException> s_instance;
    
    static void SetUpTestCase()
    {
        s_instance = System::MakeObject<::NUnitTestsPorting::ExpectedException>();
    };
    
    static void TearDownTestCase()
    {
        s_instance = nullptr;
    };
    
};

System::SharedPtr<::NUnitTestsPorting::ExpectedException> ExpectedException::s_instance;

} // namespace gtest_test

void ExpectedException::TestMethod1()
{
}

namespace gtest_test
{

TEST_F(ExpectedException, TestMethod1)
{
    ASSERT_ANY_THROW({
        s_instance->TestMethod1();
    });
}

} // namespace gtest_test

void ExpectedException::TestMethod2()
{
}

namespace gtest_test
{

TEST_F(ExpectedException, TestMethod2)
{
    ASSERT_THROW({
        s_instance->TestMethod2();
    }, System::InvalidOperationException);
}

} // namespace gtest_test

void ExpectedException::TestMethod3()
{
}

namespace gtest_test
{

TEST_F(ExpectedException, TestMethod3)
{
    ASSERT_THROW({
        s_instance->TestMethod3();
    }, System::InvalidOperationException);
}

} // namespace gtest_test

void ExpectedException::TestMethod4()
{
}

namespace gtest_test
{

TEST_F(ExpectedException, TestMethod4)
{
    ASSERT_THROW({
        s_instance->TestMethod4();
    }, System::InvalidOperationException);
}

} // namespace gtest_test

void ExpectedException::TestMethod5()
{
}

namespace gtest_test
{

TEST_F(ExpectedException, TestMethod5)
{
    ASSERT_THROW({
        s_instance->TestMethod5();
    }, System::InvalidOperationException);
}

} // namespace gtest_test

void ExpectedException::TestMethod6()
{
}

namespace gtest_test
{

TEST_F(ExpectedException, TestMethod6)
{
    ASSERT_THROW({
        s_instance->TestMethod6();
    }, System::InvalidOperationException);
}

} // namespace gtest_test

void ExpectedException::TestMethod7()
{
}

namespace gtest_test
{

TEST_F(ExpectedException, TestMethod7)
{
    ASSERT_THROW({
        s_instance->TestMethod7();
    }, System::InvalidOperationException);
}

} // namespace gtest_test

} // namespace NUnitTestsPorting

{{< /highlight >}}
