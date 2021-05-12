---
date: "2021-05-09"
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
lastmod: "2021-05-09"
weight: "1"
---

This example demonstrates how static class constructor is ported to C++. A static field with special name s_constructor__ of special type __StaticConstructor__ is added. The code is executed on program startup and this behavior is different from C# static constructors which are called right before first class usage.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassStaticConstructor
    {
        static ClassStaticConstructor()
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

} // namespace MembersPorting

{{< /highlight >}}
