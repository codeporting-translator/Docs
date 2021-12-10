---
date: "2021-12-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassFinalizers"
linktitle: "ClassFinalizers"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-12-09"
weight: "1"
---

This example demonstrates how class finalizer is ported to C++. In C++ it becomes class destructor.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassFinalizers
    {
        ~ClassFinalizers()
        {
            /* do nothing */
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

class ClassFinalizers : public System::Object
{
    typedef ClassFinalizers ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    virtual ~ClassFinalizers();
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassFinalizers.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(3834420943u, ::MembersPorting::ClassFinalizers, ThisTypeBaseTypesInfo);

ClassFinalizers::~ClassFinalizers()
{
    /* do nothing */
}

} // namespace MembersPorting

{{< /highlight >}}
