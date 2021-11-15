---
date: "2021-11-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "SimpleInterface"
linktitle: "SimpleInterface"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-11-09"
weight: "1"
---

This example demonstrates how interfaces are ported to C++. They become C++ classes which are inherited from System::Object and have RTTI declared.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public interface ISimpleInterface
    {
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>

namespace TypesPorting {

class ISimpleInterface : public System::Object
{
    typedef ISimpleInterface ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ISimpleInterface.h"

namespace TypesPorting {

RTTI_INFO_IMPL_HASH(2998900006u, ::TypesPorting::ISimpleInterface, ThisTypeBaseTypesInfo);

} // namespace TypesPorting

{{< /highlight >}}
