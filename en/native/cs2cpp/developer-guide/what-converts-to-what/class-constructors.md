---
date: "2022-04-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassConstructors"
linktitle: "ClassConstructors"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-04-09"
weight: "1"
---

This example demonstrates how class constructors are ported to C++. Public, protected and private constructors preserve their accessibility level. Internal constructors become protected.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassConstructors
    {
        public ClassConstructors()
        {
        }

        public ClassConstructors(string value)
        {
        }

        internal ClassConstructors(int value)
        {
        }

        protected ClassConstructors(bool value)
        {
        }

        private ClassConstructors(double value)
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
#include <cstdint>

namespace System
{
class String;
} // namespace System

namespace MembersPorting {

class ClassConstructors : public System::Object
{
    typedef ClassConstructors ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    ClassConstructors();
    ClassConstructors(System::String value);
    
protected:

    ClassConstructors(int32_t value);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(ClassConstructors, CODEPORTING_ARGS(int32_t value));
    
    ClassConstructors(bool value);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(ClassConstructors, CODEPORTING_ARGS(bool value));
    
private:

    ClassConstructors(double value);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(ClassConstructors, CODEPORTING_ARGS(double value));
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassConstructors.h"

#include <system/string.h>

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(2935908457u, ::MembersPorting::ClassConstructors, ThisTypeBaseTypesInfo);

ClassConstructors::ClassConstructors()
{
}

ClassConstructors::ClassConstructors(System::String value)
{
}

ClassConstructors::ClassConstructors(int32_t value)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(ClassConstructors, CODEPORTING_ARGS(int32_t value), CODEPORTING_ARGS(value));

ClassConstructors::ClassConstructors(bool value)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(ClassConstructors, CODEPORTING_ARGS(bool value), CODEPORTING_ARGS(value));

ClassConstructors::ClassConstructors(double value)
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(ClassConstructors, CODEPORTING_ARGS(double value), CODEPORTING_ARGS(value));

} // namespace MembersPorting

{{< /highlight >}}
