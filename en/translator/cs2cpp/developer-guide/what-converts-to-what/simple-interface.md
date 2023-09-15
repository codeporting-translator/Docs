---
date: "2023-09-13"
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
lastmod: "2023-09-13"
weight: "1"
---

This example demonstrates how interfaces are translated to C++. They become C++ classes which are inherited from System::Object and have RTTI declared.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public interface ISimpleInterface
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

class ISimpleInterface : public virtual System::Object
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
