---
date: "2021-04-27"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "Exceptions"
linktitle: "Exceptions"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-04-27"
weight: "1"
---

This example demonstrates how exceptions are ported to C++. They become C++ classes which are inherited from System::Exception.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;
using System.Runtime.Serialization;

namespace TypesPorting
{
    public class SimpleCustomException : Exception
    {
    }

    public class CustomMessageException : Exception
    {
        public CustomMessageException()
        {
        }

        public CustomMessageException(string message) : base(message)
        {
        }
    }

    public class CustomMessageInnerException : Exception
    {
        public CustomMessageInnerException()
        {
        }

        public CustomMessageInnerException(string message) : base(message)
        {
        }

        public CustomMessageInnerException(string message, Exception innerException) : base(message, innerException)
        {
        }
    }

    internal class BaseException : Exception
    {
        public BaseException() { }
        protected BaseException(SerializationInfo info, StreamingContext context) : base(info,context) { }
    }

    internal class BadArgumentException : BaseException
    {
        //now generating 'friend class Details_BadArgumentException;' on BaseException without forward declaration
        //for 'friend class Details_BadArgumentException;' compilier need forward declaration Details_BadArgumentException
        //so forward declaration Details_BadArgumentException and alias BadArgumentException before defining need to take up
        protected BadArgumentException(SerializationInfo info, StreamingContext context) : base(info, context) { }
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/string.h>
#include <system/runtime/serialization/streaming_context.h>
#include <system/exceptions.h>

namespace TypesPorting { class Details_BadArgumentException; using BadArgumentException = System::ExceptionWrapper<Details_BadArgumentException>; }
namespace System { namespace Runtime { namespace Serialization { class SerializationInfo; } } }

namespace TypesPorting {

class Details_SimpleCustomException;
using SimpleCustomException = System::ExceptionWrapper<Details_SimpleCustomException>;

class Details_SimpleCustomException : public System::Details_Exception
{
    typedef Details_SimpleCustomException ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_SimpleCustomException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_SimpleCustomException, CODEPORTING_ARGS());
    
};

class Details_CustomMessageException;
using CustomMessageException = System::ExceptionWrapper<Details_CustomMessageException>;

class Details_CustomMessageException : public System::Details_Exception
{
    typedef Details_CustomMessageException ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_CustomMessageException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_CustomMessageException, CODEPORTING_ARGS());
    
    Details_CustomMessageException(System::String message);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_CustomMessageException, CODEPORTING_ARGS(System::String message));
    
};

class Details_CustomMessageInnerException;
using CustomMessageInnerException = System::ExceptionWrapper<Details_CustomMessageInnerException>;

class Details_CustomMessageInnerException : public System::Details_Exception
{
    typedef Details_CustomMessageInnerException ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_CustomMessageInnerException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_CustomMessageInnerException, CODEPORTING_ARGS());
    
    Details_CustomMessageInnerException(System::String message);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_CustomMessageInnerException, CODEPORTING_ARGS(System::String message));
    
    Details_CustomMessageInnerException(System::String message, System::Exception innerException);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_CustomMessageInnerException, CODEPORTING_ARGS(System::String message, System::Exception innerException));
    
};

class Details_BaseException;
using BaseException = System::ExceptionWrapper<Details_BaseException>;

class Details_BaseException : public System::Details_Exception
{
    typedef Details_BaseException ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class Details_BadArgumentException;
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_BaseException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_BaseException, CODEPORTING_ARGS());
    
    Details_BaseException(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_BaseException, CODEPORTING_ARGS(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context));
    
};

class Details_BadArgumentException : public Details_BaseException
{
    typedef Details_BadArgumentException ThisType;
    typedef Details_BaseException BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_BadArgumentException(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_BadArgumentException, CODEPORTING_ARGS(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context));
    
    Details_BadArgumentException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_BadArgumentException, CODEPORTING_ARGS());
    
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "Exceptions.h"

#include <system/runtime/serialization/serialization_info.h>

namespace TypesPorting {

RTTI_INFO_IMPL_HASH_NAMED(3462569092u, ::TypesPorting::Details_SimpleCustomException, "TypesPorting::SimpleCustomException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_SimpleCustomException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_SimpleCustomException>(self);
}

Details_SimpleCustomException::Details_SimpleCustomException() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_SimpleCustomException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(2829839425u, ::TypesPorting::Details_CustomMessageException, "TypesPorting::CustomMessageException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_CustomMessageException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_CustomMessageException>(self);
}

Details_CustomMessageException::Details_CustomMessageException()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_CustomMessageException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

Details_CustomMessageException::Details_CustomMessageException(System::String message)
     : System::Details_Exception(message)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_CustomMessageException, CODEPORTING_ARGS(System::String message), CODEPORTING_ARGS(message));

RTTI_INFO_IMPL_HASH_NAMED(2464976903u, ::TypesPorting::Details_CustomMessageInnerException, "TypesPorting::CustomMessageInnerException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_CustomMessageInnerException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_CustomMessageInnerException>(self);
}

Details_CustomMessageInnerException::Details_CustomMessageInnerException()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_CustomMessageInnerException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

Details_CustomMessageInnerException::Details_CustomMessageInnerException(System::String message)
     : System::Details_Exception(message)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_CustomMessageInnerException, CODEPORTING_ARGS(System::String message), CODEPORTING_ARGS(message));

Details_CustomMessageInnerException::Details_CustomMessageInnerException(System::String message, System::Exception innerException)
     : System::Details_Exception(message, innerException)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_CustomMessageInnerException, CODEPORTING_ARGS(System::String message, System::Exception innerException), CODEPORTING_ARGS(message,innerException));

RTTI_INFO_IMPL_HASH_NAMED(2166308278u, ::TypesPorting::Details_BaseException, "TypesPorting::BaseException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_BaseException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_BaseException>(self);
}

Details_BaseException::Details_BaseException()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_BaseException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

Details_BaseException::Details_BaseException(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context)
     : System::Details_Exception(info, context)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_BaseException, CODEPORTING_ARGS(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context), CODEPORTING_ARGS(info,context));

RTTI_INFO_IMPL_HASH_NAMED(125447829u, ::TypesPorting::Details_BadArgumentException, "TypesPorting::BadArgumentException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_BadArgumentException::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_BadArgumentException>(self);
}

Details_BadArgumentException::Details_BadArgumentException(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context)
     : Details_BaseException(info, context)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_BadArgumentException, CODEPORTING_ARGS(System::SharedPtr<System::Runtime::Serialization::SerializationInfo> info, System::Runtime::Serialization::StreamingContext context), CODEPORTING_ARGS(info,context));

Details_BadArgumentException::Details_BadArgumentException() : Details_BaseException()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_BadArgumentException, CODEPORTING_ARGS(), CODEPORTING_ARGS());

} // namespace TypesPorting

{{< /highlight >}}
