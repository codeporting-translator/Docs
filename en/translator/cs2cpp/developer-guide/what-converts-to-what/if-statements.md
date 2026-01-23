---
date: "2026-01-16"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "IfStatements"
linktitle: "IfStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2026-01-16"
weight: "1"
---

This example demonstrates how if statement is translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class IfStatements
    {
        public void If(int value)
        {
            if (value >= 666)
                Console.WriteLine("iddqd");
        }

        public void IfElse(int value)
        {
            if (value <= 666)
                Console.WriteLine("iddqd");
            else
                Console.WriteLine("idkfa");
        }

        public void SeveralIfElse(int value)
        {
            if (value < 3)
                Console.WriteLine("iddqd");
            else if (value < 13)
                Console.WriteLine("idkfa");
            else if (value < 33)
                Console.WriteLine("idclip");
            else if (value < 666)
                Console.WriteLine("impulse 666");
            else
                Console.WriteLine("duke nukem must die");
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

class IfStatements : public System::Object
{
    typedef IfStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void If(int32_t value);
    void IfElse(int32_t value);
    void SeveralIfElse(int32_t value);
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "IfStatements.h"

#include <system/string.h>
#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(4047587550u, ::StatementsPorting::IfStatements, ThisTypeBaseTypesInfo);

void IfStatements::If(int32_t value)
{
    if (value >= 666)
    {
        System::Console::WriteLine(u"iddqd");
    }
}

void IfStatements::IfElse(int32_t value)
{
    if (value <= 666)
    {
        System::Console::WriteLine(u"iddqd");
    }
    else
    {
        System::Console::WriteLine(u"idkfa");
    }
}

void IfStatements::SeveralIfElse(int32_t value)
{
    if (value < 3)
    {
        System::Console::WriteLine(u"iddqd");
    }
    else if (value < 13)
    {
        System::Console::WriteLine(u"idkfa");
    }
    else if (value < 33)
    {
        System::Console::WriteLine(u"idclip");
    }
    else if (value < 666)
    {
        System::Console::WriteLine(u"impulse 666");
    }
    else
    {
        System::Console::WriteLine(u"duke nukem must die");
    }
}

} // namespace StatementsPorting

{{< /highlight >}}
