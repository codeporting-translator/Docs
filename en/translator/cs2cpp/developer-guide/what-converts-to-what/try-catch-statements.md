---
date: "2022-10-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "TryCatchStatements"
linktitle: "TryCatchStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-10-09"
weight: "1"
---

This example demonstrates how try-catch statement is translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: -o finally_statement_as_lambda=true.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class GrandParentException : Exception
    {
    }

    public class ParentException : GrandParentException
    {
    }

    public class ChildException : ParentException
    {
    }

    public class TryCatchStatements
    {
        public void TryCatch()
        {
            try
            {
                InnerMethod();
            }
            catch (ChildException)
            {
                Console.WriteLine("Catch ChildException");
            }
            catch (ParentException)
            {
                Console.WriteLine("Catch ParentException");
            }
            catch (GrandParentException)
            {
                Console.WriteLine("Catch GrandParentException");
            }
            catch (Exception)
            {
                Console.WriteLine("Catch Exception");
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

namespace StatementsPorting {

class Details_GrandParentException;
using GrandParentException = System::ExceptionWrapper<Details_GrandParentException>;

class Details_GrandParentException : public System::Details_Exception
{
    typedef Details_GrandParentException ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_GrandParentException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_GrandParentException, CODEPORTING_ARGS());
    
};

class Details_ParentException;
using ParentException = System::ExceptionWrapper<Details_ParentException>;

class Details_ParentException : public Details_GrandParentException
{
    typedef Details_ParentException ThisType;
    typedef Details_GrandParentException BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_ParentException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_ParentException, CODEPORTING_ARGS());
    
};

class Details_ChildException;
using ChildException = System::ExceptionWrapper<Details_ChildException>;

class Details_ChildException : public Details_ParentException
{
    typedef Details_ChildException ThisType;
    typedef Details_ParentException BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_ChildException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_ChildException, CODEPORTING_ARGS());
    
};

class TryCatchStatements : public System::Object
{
    typedef TryCatchStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void TryCatch();
    
private:

    void InnerMethod();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "TryCatchStatements.h"

#include <system/string.h>
#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH_NAMED(3457991606u, ::StatementsPorting::Details_GrandParentException, "StatementsPorting::GrandParentException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_GrandParentException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_GrandParentException>(self);
}

Details_GrandParentException::Details_GrandParentException() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_GrandParentException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(3209790888u, ::StatementsPorting::Details_ParentException, "StatementsPorting::ParentException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_ParentException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_ParentException>(self);
}

Details_ParentException::Details_ParentException() : Details_GrandParentException()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_ParentException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(2858341424u, ::StatementsPorting::Details_ChildException, "StatementsPorting::ChildException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_ChildException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_ChildException>(self);
}

Details_ChildException::Details_ChildException() : Details_ParentException()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_ChildException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH(808916801u, ::StatementsPorting::TryCatchStatements, ThisTypeBaseTypesInfo);

void TryCatchStatements::TryCatch()
{
    try
    {
        InnerMethod();
    }
    catch (ChildException& )
    {
        System::Console::WriteLine(u"Catch ChildException");
    }
    catch (ParentException& )
    {
        System::Console::WriteLine(u"Catch ParentException");
    }
    catch (GrandParentException& )
    {
        System::Console::WriteLine(u"Catch GrandParentException");
    }
    catch (System::Exception& )
    {
        System::Console::WriteLine(u"Catch Exception");
    }
    
}

void TryCatchStatements::InnerMethod()
{
}

} // namespace StatementsPorting

{{< /highlight >}}
