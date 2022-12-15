---
date: "2022-12-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "SimpleStruct"
linktitle: "SimpleStruct"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-12-09"
weight: "1"
---

This example demonstrates how simple C# struct is translated to C++. A result C++ class inherits System::Object class and contains definitions for RTTI. 

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public struct SimpleStruct
    {
    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>

namespace TypesPorting {

class SimpleStruct : public System::Object, public System::Details::BoxableObjectBase
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
