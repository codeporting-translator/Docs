---
date: "2024-10-11"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "TryFinallyStatements"
linktitle: "TryFinallyStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2024-10-11"
weight: "1"
---

This example demonstrates how try-finally statement is translated to C++. They are translated to lambda expressions passed to System::DoTryFinally function.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: -o finally_statement_as_lambda=true.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class TryFinallyStatements
    {
        public void TryFinally()
        {
            try
            {
                InnerMethod();
            }
            finally
            {
                Console.WriteLine("Finally");
            }
        }

        public void TryFinallyWithException()
        {
            try
            {
                InnerMethod();
            }
            finally
            {
                Console.WriteLine("Finally");
                throw new Exception();
            }
        }

        public void EnclosedTryFinally()
        {
            try
            {
                try
                {
                    InnerMethod();
                }
                finally
                {
                    Console.WriteLine("Inner finally");
                }
            }
            finally
            {
                Console.WriteLine("Outer finally");
            }
        }

        public void EnclosedTryFinallyWithException()
        {
            try
            {
                try
                {
                    InnerMethod();
                }
                finally
                {
                    Console.WriteLine("Inner finally");
                    throw new Exception();
                }
            }
            finally
            {
                Console.WriteLine("Outer finally");
            }
        }
        
        public int ValueReturnTry()
        {
            try
            {
                return 1;
            }
            finally
            {
                Console.WriteLine("finally");
            }
        }

        public void VoidReturnTry()
        {
            try
            {
                return;
            }
            finally
            {
                Console.WriteLine("finally");
            }
        }

        public int PropertyGetTry
        {
            get
            {
                try
                {
                    return 1;
                }
                finally
                {
                    Console.WriteLine("finally");
                }
            }
        }

        private void InnerMethod()
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
#include <cstdint>

namespace StatementsPorting {

class TryFinallyStatements : public System::Object
{
    typedef TryFinallyStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    int32_t get_PropertyGetTry();
    
    void TryFinally();
    void TryFinallyWithException();
    void EnclosedTryFinally();
    void EnclosedTryFinallyWithException();
    int32_t ValueReturnTry();
    void VoidReturnTry();
    
private:

    void InnerMethod();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "TryFinallyStatements.h"

#include <system/string.h>
#include <system/exceptions.h>
#include <system/do_try_finally.h>
#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2690213833u, ::StatementsPorting::TryFinallyStatements, ThisTypeBaseTypesInfo);

// Try-finally statement is translated using System::DoTryFinally function. Reference to this function is always valid in runtime.
// We block the warnings related as these are false alarms (the exception, if caught, will be re-thrown from the destructor).
#if defined(__MSVC__)
#pragma warning( push )
#pragma warning(disable : 4715)
#pragma warning(disable : 4700)
#pragma warning(disable : 4701)
#elif defined(__GNUC__)
#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wreturn-type"
#endif
int32_t TryFinallyStatements::get_PropertyGetTry()
{
    auto optionalReturnValue__99 = System::DoTryFinally(
    [&](bool& isReturned__99) -> int32_t /* try-catch block */ 
    {
        return 1;
        isReturned__99 = false;
        return System::Details::initialized_value;
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"finally");
    });
    if (optionalReturnValue__99) return *optionalReturnValue__99;
    
}
#if defined(__MSVC__)
#pragma warning( pop )
#elif defined(__GNUC__)
#pragma GCC diagnostic pop
#endif

void TryFinallyStatements::TryFinally()
{
    System::DoTryFinally([&] /* try-catch block */ 
    {
        InnerMethod();
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    
}

void TryFinallyStatements::TryFinallyWithException()
{
    System::DoTryFinally([&] /* try-catch block */ 
    {
        InnerMethod();
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
        throw System::Exception();
    });
    
}

void TryFinallyStatements::EnclosedTryFinally()
{
    System::DoTryFinally([&] /* try-catch block */ 
    {
        System::DoTryFinally([&] /* try-catch block */ 
            {
                InnerMethod();
            }
            , [&] /* finally block */ 
            {
                System::Console::WriteLine(u"Inner finally");
            });
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Outer finally");
    });
    
}

void TryFinallyStatements::EnclosedTryFinallyWithException()
{
    System::DoTryFinally([&] /* try-catch block */ 
    {
        System::DoTryFinally([&] /* try-catch block */ 
            {
                InnerMethod();
            }
            , [&] /* finally block */ 
            {
                System::Console::WriteLine(u"Inner finally");
                throw System::Exception();
            });
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Outer finally");
    });
    
}

// Try-finally statement is translated using System::DoTryFinally function. Reference to this function is always valid in runtime.
// We block the warnings related as these are false alarms (the exception, if caught, will be re-thrown from the destructor).
#if defined(__MSVC__)
#pragma warning( push )
#pragma warning(disable : 4715)
#pragma warning(disable : 4700)
#pragma warning(disable : 4701)
#elif defined(__GNUC__)
#pragma GCC diagnostic push
#pragma GCC diagnostic ignored "-Wreturn-type"
#endif
int32_t TryFinallyStatements::ValueReturnTry()
{
    auto optionalReturnValue__73 = System::DoTryFinally(
    [&](bool& isReturned__73) -> int32_t /* try-catch block */ 
    {
        return 1;
        isReturned__73 = false;
        return System::Details::initialized_value;
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"finally");
    });
    if (optionalReturnValue__73) return *optionalReturnValue__73;
    
}
#if defined(__MSVC__)
#pragma warning( pop )
#elif defined(__GNUC__)
#pragma GCC diagnostic pop
#endif

void TryFinallyStatements::VoidReturnTry()
{
    bool optionalReturnValue__85 = System::DoTryFinally(
    [&](bool& isReturned__85) -> void /* try-catch block */ 
    {
        return;
        isReturned__85 = false;
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"finally");
    });
    if (optionalReturnValue__85) return;
    
}

void TryFinallyStatements::InnerMethod()
{
}

} // namespace StatementsPorting

{{< /highlight >}}
