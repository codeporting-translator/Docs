---
date: "2024-06-13"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "SimpleClass"
linktitle: "SimpleClass"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2024-06-13"
weight: "1"
---

This example demonstrates how simple C# class is translated to C++. A result C++ class inherits System::Object class and contains definitions for RTTI.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public class SimpleClass
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

class SimpleClass : public System::Object
{
    typedef SimpleClass ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "SimpleClass.h"

namespace TypesPorting {

RTTI_INFO_IMPL_HASH(3993001598u, ::TypesPorting::SimpleClass, ThisTypeBaseTypesInfo);

} // namespace TypesPorting

{{< /highlight >}}
