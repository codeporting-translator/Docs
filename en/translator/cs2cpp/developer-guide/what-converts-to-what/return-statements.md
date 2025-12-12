---
date: "2025-12-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ReturnStatements"
linktitle: "ReturnStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2025-12-10"
weight: "1"
---

This example demonstrates how return statement is translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class ReturnStatements
    {
        public void ReturnVoid(int value)
        {
            if (value < 10)
                return;
            Console.WriteLine(value);
        }

        public int ReturnValue(int value)
        {
            if (value < 10)
                return 666;
            Console.WriteLine(value);
            return 13;
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

namespace StatementsPorting {

class ReturnStatements : public System::Object
{
    typedef ReturnStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void ReturnVoid(int32_t value);
    int32_t ReturnValue(int32_t value);
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ReturnStatements.h"

#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2520855697u, ::StatementsPorting::ReturnStatements, ThisTypeBaseTypesInfo);

void ReturnStatements::ReturnVoid(int32_t value)
{
    if (value < 10)
    {
        return;
    }
    System::Console::WriteLine(value);
}

int32_t ReturnStatements::ReturnValue(int32_t value)
{
    if (value < 10)
    {
        return 666;
    }
    System::Console::WriteLine(value);
    return 13;
}

} // namespace StatementsPorting

{{< /highlight >}}
