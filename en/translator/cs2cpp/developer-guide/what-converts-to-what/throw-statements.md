---
date: "2023-11-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ThrowStatements"
linktitle: "ThrowStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2023-11-10"
weight: "1"
---

This example demonstrates how throw statement is translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class SomeException : Exception
    {
    }

    public class OtherException : Exception
    {
    }

    public class ThrowStatements
    {
        public void Throw(int value)
        {
            if (value < 10)
            {
                Console.WriteLine("Too small value");
                throw new SomeException();
            }
            if (value > 20)
            {
                Console.WriteLine("Too big value");
                throw new OtherException();
            }
        }

        public void RethrowSame()
        {
            try
            {
                InnerMethod();
            }
            catch (SomeException)
            {
                Console.WriteLine("Catch SomeException");
                throw;
            }
        }

        public void RethrowOther()
        {
            try
            {
                InnerMethod();
            }
            catch (SomeException)
            {
                Console.WriteLine("Catch SomeException");
                throw new OtherException();
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
#include <system/exceptions.h>
#include <cstdint>

namespace StatementsPorting {

class Details_SomeException;
using SomeException = System::ExceptionWrapper<Details_SomeException>;

class Details_SomeException : public System::Details_Exception
{
    typedef Details_SomeException ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_SomeException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_SomeException, CODEPORTING_ARGS());
    
};

class Details_OtherException;
using OtherException = System::ExceptionWrapper<Details_OtherException>;

class Details_OtherException : public System::Details_Exception
{
    typedef Details_OtherException ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_OtherException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_OtherException, CODEPORTING_ARGS());
    
};

class ThrowStatements : public System::Object
{
    typedef ThrowStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void Throw(int32_t value);
    void RethrowSame();
    void RethrowOther();
    
private:

    void InnerMethod();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ThrowStatements.h"

#include <system/string.h>
#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH_NAMED(927407390u, ::StatementsPorting::Details_SomeException, "StatementsPorting::SomeException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_SomeException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_SomeException>(self);
}

Details_SomeException::Details_SomeException() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_SomeException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(1983333724u, ::StatementsPorting::Details_OtherException, "StatementsPorting::OtherException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_OtherException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_OtherException>(self);
}

Details_OtherException::Details_OtherException() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_OtherException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH(922865773u, ::StatementsPorting::ThrowStatements, ThisTypeBaseTypesInfo);

void ThrowStatements::Throw(int32_t value)
{
    if (value < 10)
    {
        System::Console::WriteLine(u"Too small value");
        throw SomeException();
    }
    if (value > 20)
    {
        System::Console::WriteLine(u"Too big value");
        throw OtherException();
    }
}

void ThrowStatements::RethrowSame()
{
    try
    {
        InnerMethod();
    }
    catch (SomeException& )
    {
        System::Console::WriteLine(u"Catch SomeException");
        throw;
    }
    
}

void ThrowStatements::RethrowOther()
{
    try
    {
        InnerMethod();
    }
    catch (SomeException& )
    {
        System::Console::WriteLine(u"Catch SomeException");
        throw OtherException();
    }
    
}

void ThrowStatements::InnerMethod()
{
}

} // namespace StatementsPorting

{{< /highlight >}}
