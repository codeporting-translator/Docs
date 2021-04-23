---
date: "2021-27-23"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: false
title: "SimpleStruct"
linktitle: "SimpleStruct"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-27-23"
weight: "1"
---

This example demonstrates how simple C# struct is ported to C++. A result C++ class inherits System::Object class and contains definitions for RTTI. 

Additional command-line options passed to CsToCppPorter: none.

## Source C# code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public struct SimpleStruct
    {
    }
}
{{< /highlight >}}

## Ported code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>

namespace TypesPorting {

class SimpleStruct : public System::Object
{
    typedef SimpleStruct ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    SimpleStruct();
    
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "SimpleStruct.h"

namespace TypesPorting {

RTTI_INFO_IMPL_HASH(2325628207u, ::TypesPorting::SimpleStruct, ThisTypeBaseTypesInfo);

SimpleStruct::SimpleStruct()
{
}

} // namespace TypesPorting

{{< /highlight >}}
