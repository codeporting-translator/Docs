---
date: "2023-08-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassVirtualMethods"
linktitle: "ClassVirtualMethods"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2023-08-10"
weight: "1"
---

This example demonstrates how virtual class methods are translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassVirtualMethods
    {
        public virtual void VirtualMethod()
        {
        }

        public virtual void OtherVirtualMethod(int value)
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

namespace MembersPorting {

class ClassVirtualMethods : public System::Object
{
    typedef ClassVirtualMethods ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    virtual void VirtualMethod();
    virtual void OtherVirtualMethod(int32_t value);
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassVirtualMethods.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(2262109527u, ::MembersPorting::ClassVirtualMethods, ThisTypeBaseTypesInfo);

void ClassVirtualMethods::VirtualMethod()
{
}

void ClassVirtualMethods::OtherVirtualMethod(int32_t value)
{
}

} // namespace MembersPorting

{{< /highlight >}}
