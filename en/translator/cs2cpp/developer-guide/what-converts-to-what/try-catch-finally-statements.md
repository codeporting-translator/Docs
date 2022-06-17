---
date: "2022-06-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "TryCatchFinallyStatements"
linktitle: "TryCatchFinallyStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-06-09"
weight: "1"
---

This example demonstrates how try-catch-finally statement is translated to C++. They are translated to lambda expressions passed to System::DoTryFinally function.

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

    public class TryCatchFinallyStatements
    {
        public void TryCatchFinally()
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
            finally
            {
                Console.WriteLine("Finally");
            }
        }

        public void EnclosedTryCatchFinally()
        {
            try
            {
                InnerMethod();
                try
                {
                    InnerMethod();
                }
                catch (InvalidOperationException)
                {
                    Console.WriteLine("Catch InvalidOperationException");
                }
                finally
                {
                    Console.WriteLine("Finally");
                }
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
            finally
            {
                Console.WriteLine("Finally");
            }
        }

        public void EnclosedTryCatchFinallyWithException()
        {
            try
            {
                InnerMethod();
                try
                {
                    InnerMethod();
                }
                catch (InvalidOperationException)
                {
                    Console.WriteLine("Catch InvalidOperationException");
                }
                finally
                {
                    Console.WriteLine("Finally");
                    throw new Exception();
                }
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
            finally
            {
                Console.WriteLine("Finally");
            }
            
            // do something
            try
            {
                //do something
            }
            catch (Exception e)
            {
                //do something
            }
            finally
            { 
                //do something
            }
            
            try
            {
            }
            catch (Exception e)
            {
            }
            finally
            {
            }
        }
        
        public void VoidReturnTryCatchFinally()
        {
            try
            {
                InnerMethod();
                return;
            }
            catch (Exception)
            {
                Console.WriteLine("Catch Exception");
            }
            finally
            {
                Console.WriteLine("Finally");
            }
        }
        
        public int ValueReturnTryCatchFinally()
        {
            try
            {
                InnerMethod();
                return 1;
            }
            catch (Exception)
            {
                Console.WriteLine("Catch Exception");
            }
            finally
            {
                Console.WriteLine("Finally");
            }

            return 1;
        }
        
        public int PropertyGetTryCatchFinally
        {
            get
            {
                try
                {
                    InnerMethod();
                    return 1;
                }
                catch (Exception)
                {
                    Console.WriteLine("Catch Exception");
                }
                finally
                {
                    Console.WriteLine("Finally");
                }
                
                return 1;
            }
        }
        
        public object ObjReturnTryCatchFinally()
        {
            try
            {
                InnerMethod();
                return null;
            }
            catch (Exception)
            {
                Console.WriteLine("Catch Exception");
            }
            finally
            {
                Console.WriteLine("Finally");
            }
            
            return null;
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

class TryCatchFinallyStatements : public System::Object
{
    typedef TryCatchFinallyStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    int32_t get_PropertyGetTryCatchFinally();
    
    void TryCatchFinally();
    void EnclosedTryCatchFinally();
    void EnclosedTryCatchFinallyWithException();
    void VoidReturnTryCatchFinally();
    int32_t ValueReturnTryCatchFinally();
    System::SharedPtr<System::Object> ObjReturnTryCatchFinally();
    
private:

    void InnerMethod();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "TryCatchFinallyStatements.h"

#include <system/string.h>
#include <system/exceptions.h>
#include <system/do_try_finally.h>
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

RTTI_INFO_IMPL_HASH(684751338u, ::StatementsPorting::TryCatchFinallyStatements, ThisTypeBaseTypesInfo);

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
int32_t TryCatchFinallyStatements::get_PropertyGetTryCatchFinally()
{
    auto optionalReturnValue__192 = System::DoTryFinally(
    [&](bool& isReturned__192) -> int32_t /* try-catch block */ 
    {
        try
        {
            InnerMethod();
            return 1;
        }
        catch (System::Exception& )
        {
            System::Console::WriteLine(u"Catch Exception");
        }
        
        isReturned__192 = false;
        return System::Details::initialized_value;
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    if (optionalReturnValue__192) return *optionalReturnValue__192;
    
    
    return 1;
}
#if defined(__MSVC__)
#pragma warning( pop )
#elif defined(__GNUC__)
#pragma GCC diagnostic pop
#endif

void TryCatchFinallyStatements::TryCatchFinally()
{
    System::DoTryFinally([&] /* try-catch block */ 
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
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    
}

void TryCatchFinallyStatements::EnclosedTryCatchFinally()
{
    System::DoTryFinally([&] /* try-catch block */ 
    {
        try
        {
            InnerMethod();
            System::DoTryFinally([&] /* try-catch block */ 
            {
                try
                {
                    InnerMethod();
                }
                catch (System::InvalidOperationException& )
                {
                    System::Console::WriteLine(u"Catch InvalidOperationException");
                }
                
            }
            , [&] /* finally block */ 
            {
                System::Console::WriteLine(u"Finally");
            });
            
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
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    
}

void TryCatchFinallyStatements::EnclosedTryCatchFinallyWithException()
{
    System::DoTryFinally([&] /* try-catch block */ 
    {
        try
        {
            InnerMethod();
            System::DoTryFinally([&] /* try-catch block */ 
            {
                try
                {
                    InnerMethod();
                }
                catch (System::InvalidOperationException& )
                {
                    System::Console::WriteLine(u"Catch InvalidOperationException");
                }
                
            }
            , [&] /* finally block */ 
            {
                System::Console::WriteLine(u"Finally");
                throw System::Exception();
            });
            
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
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    
    
    // do something
    System::DoTryFinally([&] /* try-catch block */ 
    {
        try
        {
            //do something
        }
        catch (System::Exception& e)
        {
            //do something
        }
        
    }
    , [&] /* finally block */ 
    {
        //do something
    });
    
    
    System::DoTryFinally([&] /* try-catch block */ 
    {
        try
        {
        }
        catch (System::Exception& e)
        {
        }
        
    }
    , [&] /* finally block */ 
    {
    });
    
}

void TryCatchFinallyStatements::VoidReturnTryCatchFinally()
{
    bool optionalReturnValue__154 = System::DoTryFinally(
    [&](bool& isReturned__154) -> void /* try-catch block */ 
    {
        try
        {
            InnerMethod();
            return;
        }
        catch (System::Exception& )
        {
            System::Console::WriteLine(u"Catch Exception");
        }
        
        isReturned__154 = false;
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    if (optionalReturnValue__154) return;
    
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
int32_t TryCatchFinallyStatements::ValueReturnTryCatchFinally()
{
    auto optionalReturnValue__171 = System::DoTryFinally(
    [&](bool& isReturned__171) -> int32_t /* try-catch block */ 
    {
        try
        {
            InnerMethod();
            return 1;
        }
        catch (System::Exception& )
        {
            System::Console::WriteLine(u"Catch Exception");
        }
        
        isReturned__171 = false;
        return System::Details::initialized_value;
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    if (optionalReturnValue__171) return *optionalReturnValue__171;
    
    
    return 1;
}
#if defined(__MSVC__)
#pragma warning( pop )
#elif defined(__GNUC__)
#pragma GCC diagnostic pop
#endif

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
System::SharedPtr<System::Object> TryCatchFinallyStatements::ObjReturnTryCatchFinally()
{
    auto optionalReturnValue__212 = System::DoTryFinally(
    [&](bool& isReturned__212) -> System::SharedPtr<System::Object> /* try-catch block */ 
    {
        try
        {
            InnerMethod();
            return nullptr;
        }
        catch (System::Exception& )
        {
            System::Console::WriteLine(u"Catch Exception");
        }
        
        isReturned__212 = false;
        return System::Details::initialized_value;
    }
    , [&] /* finally block */ 
    {
        System::Console::WriteLine(u"Finally");
    });
    if (optionalReturnValue__212) return *optionalReturnValue__212;
    
    
    return nullptr;
}
#if defined(__MSVC__)
#pragma warning( pop )
#elif defined(__GNUC__)
#pragma GCC diagnostic pop
#endif

void TryCatchFinallyStatements::InnerMethod()
{
}

} // namespace StatementsPorting

{{< /highlight >}}
