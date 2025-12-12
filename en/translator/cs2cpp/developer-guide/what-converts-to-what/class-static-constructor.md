---
date: "2025-12-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassStaticConstructor"
linktitle: "ClassStaticConstructor"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2025-12-10"
weight: "1"
---

This example demonstrates how static class constructor is translated to C++. A static field with special name s_constructor__ of special type __StaticConstructor__ is added. The code is executed on program startup and this behavior is different from C# static constructors which are called right before first class usage.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using NUnit.Framework;


namespace MembersPorting
{
    public class ClassStaticConstructor
    {
        static ClassStaticConstructor()
        {
        }
    }

    public static class DeletedConstructor
    {
        static void Foo() { }
    }

    public static class StaticConstructorExists
    {
        static readonly bool Foo;

        static void Bar() { }

        static StaticConstructorExists()
        {
            Foo = false;
        }
    }

    [TestFixture]
    public static class NonDeletedConstructor { }

    [CodePorting.Translator.Cs2Cpp.CppDeferredInit]
    internal static class SlidesIssue
    {
        static SlidesIssue() {}
    }
}

{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <mutex>
#include <memory>
#include <gtest/gtest.h>

namespace MembersPorting {

class ClassStaticConstructor : public System::Object
{
    typedef ClassStaticConstructor ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
private:

    static struct __StaticConstructor__ { __StaticConstructor__(); } s_constructor__;
    
};

class DeletedConstructor
{
    typedef DeletedConstructor ThisType;
    
private:

    static void Foo();
    
public:
    DeletedConstructor() = delete;
};

class StaticConstructorExists
{
    typedef StaticConstructorExists ThisType;
    
private:

    static bool Foo;
    
    static void Bar();
    
    static struct __StaticConstructor__ { __StaticConstructor__(); } s_constructor__;
    
public:
    StaticConstructorExists() = delete;
};

class NonDeletedConstructor
{
    typedef NonDeletedConstructor ThisType;
    
};

class SlidesIssue
{
    typedef SlidesIssue ThisType;
    
public:

    SlidesIssue();
    
private:

    static void __StaticConstructor__();
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassStaticConstructor.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(2828145692u, ::MembersPorting::ClassStaticConstructor, ThisTypeBaseTypesInfo);

ClassStaticConstructor::__StaticConstructor__ ClassStaticConstructor::s_constructor__;

ClassStaticConstructor::__StaticConstructor__::__StaticConstructor__()
{
}

void DeletedConstructor::Foo()
{
}

bool StaticConstructorExists::Foo = false;

StaticConstructorExists::__StaticConstructor__ StaticConstructorExists::s_constructor__;

void StaticConstructorExists::Bar()
{
}

StaticConstructorExists::__StaticConstructor__::__StaticConstructor__()
{
    StaticConstructorExists::Foo = false;
}

namespace gtest_test
{

class NonDeletedConstructor : public ::testing::Test
{
protected:
    static void SetUpTestCase()
    {
    };
    
    static void TearDownTestCase()
    {
    };
    
};

} // namespace gtest_test

void SlidesIssue::__StaticConstructor__()
{
    thread_local static bool inProgress = false;
    if (inProgress) return;
    static const auto markFinished = [](bool *inProgress) { *inProgress = false; };
    inProgress = true;
    const std::unique_ptr<bool, decltype(markFinished)> finish(&inProgress, markFinished);
    
    static std::once_flag once;
    std::call_once(once, []
    {
    });
}

SlidesIssue::SlidesIssue()
{
    __StaticConstructor__();
    
}

} // namespace MembersPorting

{{< /highlight >}}
