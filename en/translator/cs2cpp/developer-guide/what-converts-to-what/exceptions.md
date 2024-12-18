---
date: "2024-12-10"
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
lastmod: "2024-12-10"
weight: "1"
---

This example demonstrates how exceptions are translated to C++. They become C++ classes which are inherited from System::Exception.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

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

    public class CustomMessageInnerExceptionInhetor : CustomMessageInnerException
    {
        CustomMessageInnerExceptionInhetor(string res)
            : base(res, null)
        { }
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

    // No fields, no ctors
    public class Exception1 : Exception
    {
    }

    // Some field, no ctors
    public class Exception2 : Exception
    {
        public int field;
    }

    // No fields, static ctor
    public class Exception3 : Exception
    {
        static Exception3() { }
    }


    // Some field, static ctor
    public class Exception4 : Exception
    {
        static readonly long field;

        static Exception4()
        {
            field = 0;
        }
    }


    // No fields, normal ctor
    public class Exception5 : Exception
    {
        public Exception5() { }
    }

    // Some field, normal ctor
    public class Exception6 : Exception
    {
        public int field;

        public Exception6()
        {
            field = -1;
        }
    }


    // No fields, static and normal ctors
    public class Exception7 : Exception
    {
        static Exception7() { }

        public Exception7() { }
    }

    // Some fields, static and normal ctors
    public class Exception8 : Exception
    {
        static readonly long field1;
        public int field2;

        static Exception8()
        {
            field1 = 0;
        }

        public Exception8()
        {
            field2 = -1;
        }

    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/exceptions.h>
#include <cstdint>

namespace System
{
namespace Runtime
{
namespace Serialization
{
class SerializationInfo;
class StreamingContext;
} // namespace Serialization
} // namespace Runtime
class String;
} // namespace System
namespace TypesPorting
{
class Details_BadArgumentException; using BadArgumentException = System::ExceptionWrapper<Details_BadArgumentException>;
} // namespace TypesPorting

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

class Details_CustomMessageInnerExceptionInhetor;
using CustomMessageInnerExceptionInhetor = System::ExceptionWrapper<Details_CustomMessageInnerExceptionInhetor>;

class Details_CustomMessageInnerExceptionInhetor : public Details_CustomMessageInnerException
{
    typedef Details_CustomMessageInnerExceptionInhetor ThisType;
    typedef Details_CustomMessageInnerException BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_CustomMessageInnerExceptionInhetor(System::String res);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_CustomMessageInnerExceptionInhetor, CODEPORTING_ARGS(System::String res));
    
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
    
};

// No fields, no ctors
class Details_Exception1;
using Exception1 = System::ExceptionWrapper<Details_Exception1>;

class Details_Exception1 : public System::Details_Exception
{
    typedef Details_Exception1 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception1();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception1, CODEPORTING_ARGS());
    
};

// Some field, no ctors
class Details_Exception2;
using Exception2 = System::ExceptionWrapper<Details_Exception2>;

class Details_Exception2 : public System::Details_Exception
{
    typedef Details_Exception2 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
public:

    int32_t field;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception2();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception2, CODEPORTING_ARGS());
    
};

// No fields, static ctor
class Details_Exception3;
using Exception3 = System::ExceptionWrapper<Details_Exception3>;

class Details_Exception3 : public System::Details_Exception
{
    typedef Details_Exception3 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception3();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception3, CODEPORTING_ARGS());
    
private:

    static struct __StaticConstructor__ { __StaticConstructor__(); } s_constructor__;
    
};

// Some field, static ctor
class Details_Exception4;
using Exception4 = System::ExceptionWrapper<Details_Exception4>;

class Details_Exception4 : public System::Details_Exception
{
    typedef Details_Exception4 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception4();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception4, CODEPORTING_ARGS());
    
private:

    static int64_t field;
    
    static struct __StaticConstructor__ { __StaticConstructor__(); } s_constructor__;
    
};

// No fields, normal ctor
class Details_Exception5;
using Exception5 = System::ExceptionWrapper<Details_Exception5>;

class Details_Exception5 : public System::Details_Exception
{
    typedef Details_Exception5 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception5();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception5, CODEPORTING_ARGS());
    
};

// Some field, normal ctor
class Details_Exception6;
using Exception6 = System::ExceptionWrapper<Details_Exception6>;

class Details_Exception6 : public System::Details_Exception
{
    typedef Details_Exception6 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
public:

    int32_t field;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception6();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception6, CODEPORTING_ARGS());
    
};

// No fields, static and normal ctors
class Details_Exception7;
using Exception7 = System::ExceptionWrapper<Details_Exception7>;

class Details_Exception7 : public System::Details_Exception
{
    typedef Details_Exception7 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception7();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception7, CODEPORTING_ARGS());
    
private:

    static struct __StaticConstructor__ { __StaticConstructor__(); } s_constructor__;
    
};

// Some fields, static and normal ctors
class Details_Exception8;
using Exception8 = System::ExceptionWrapper<Details_Exception8>;

class Details_Exception8 : public System::Details_Exception
{
    typedef Details_Exception8 ThisType;
    typedef System::Details_Exception BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;
    
public:

    int32_t field2;
    
protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_Exception8();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_Exception8, CODEPORTING_ARGS());
    
private:

    static int64_t field1;
    
    static struct __StaticConstructor__ { __StaticConstructor__(); } s_constructor__;
    
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "Exceptions.h"

#include <system/string.h>
#include <system/runtime/serialization/streaming_context.h>
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

RTTI_INFO_IMPL_HASH_NAMED(4188024334u, ::TypesPorting::Details_CustomMessageInnerExceptionInhetor, "TypesPorting::CustomMessageInnerExceptionInhetor", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_CustomMessageInnerExceptionInhetor::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_CustomMessageInnerExceptionInhetor>(self);
}

Details_CustomMessageInnerExceptionInhetor::Details_CustomMessageInnerExceptionInhetor(System::String res)
    : Details_CustomMessageInnerException(res, nullptr)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_CustomMessageInnerExceptionInhetor, CODEPORTING_ARGS(System::String res), CODEPORTING_ARGS(res));

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

RTTI_INFO_IMPL_HASH_NAMED(731907242u, ::TypesPorting::Details_Exception1, "TypesPorting::Exception1", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_Exception1::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception1>(self);
}

Details_Exception1::Details_Exception1() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception1, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(731907243u, ::TypesPorting::Details_Exception2, "TypesPorting::Exception2", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_Exception2::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception2>(self);
}

Details_Exception2::Details_Exception2() : System::Details_Exception(), field(0)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception2, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(731907244u, ::TypesPorting::Details_Exception3, "TypesPorting::Exception3", ThisTypeBaseTypesInfo);

Details_Exception3::__StaticConstructor__ Details_Exception3::s_constructor__;

[[noreturn]] void Details_Exception3::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception3>(self);
}

Details_Exception3::__StaticConstructor__::__StaticConstructor__()
{
}

Details_Exception3::Details_Exception3() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception3, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(731907245u, ::TypesPorting::Details_Exception4, "TypesPorting::Exception4", ThisTypeBaseTypesInfo);

int64_t Details_Exception4::field = 0;

Details_Exception4::__StaticConstructor__ Details_Exception4::s_constructor__;

[[noreturn]] void Details_Exception4::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception4>(self);
}

Details_Exception4::__StaticConstructor__::__StaticConstructor__()
{
    Details_Exception4::field = 0;
}

Details_Exception4::Details_Exception4() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception4, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(731907246u, ::TypesPorting::Details_Exception5, "TypesPorting::Exception5", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_Exception5::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception5>(self);
}

Details_Exception5::Details_Exception5()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception5, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(731907247u, ::TypesPorting::Details_Exception6, "TypesPorting::Exception6", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_Exception6::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception6>(self);
}

Details_Exception6::Details_Exception6() : field(0)
{
    field = -1;
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception6, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(731907248u, ::TypesPorting::Details_Exception7, "TypesPorting::Exception7", ThisTypeBaseTypesInfo);

Details_Exception7::__StaticConstructor__ Details_Exception7::s_constructor__;

[[noreturn]] void Details_Exception7::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception7>(self);
}

Details_Exception7::__StaticConstructor__::__StaticConstructor__()
{
}

Details_Exception7::Details_Exception7()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception7, CODEPORTING_ARGS(), CODEPORTING_ARGS());

RTTI_INFO_IMPL_HASH_NAMED(731907249u, ::TypesPorting::Details_Exception8, "TypesPorting::Exception8", ThisTypeBaseTypesInfo);

int64_t Details_Exception8::field1 = 0;

Details_Exception8::__StaticConstructor__ Details_Exception8::s_constructor__;

[[noreturn]] void Details_Exception8::DoThrow(const System::ExceptionPtr& self) const
{
    throw System::ExceptionWrapper<Details_Exception8>(self);
}

Details_Exception8::__StaticConstructor__::__StaticConstructor__()
{
    Details_Exception8::field1 = 0;
}

Details_Exception8::Details_Exception8() : field2(0)
{
    field2 = -1;
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_Exception8, CODEPORTING_ARGS(), CODEPORTING_ARGS());

} // namespace TypesPorting

{{< /highlight >}}
