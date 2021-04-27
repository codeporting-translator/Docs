---
date: "2021-04-27"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: false
title: "WhileStatements"
linktitle: "WhileStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-04-27"
weight: "1"
---

This example demonstrates how while statement is ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class WhileStatements
    {
        public void While(int max)
        {
            int number = 0;
            while (number < max)
            {
                Console.WriteLine(number);
                ++number;
            }
        }

        public void EnclosedWhile(int max1, int max2)
        {
            int number1 = 0;
            while (number1 < max1)
            {
                int number2 = 0;
                while (number2 < max2)
                {
                    Console.WriteLine(number1 + number2);
                    ++ number2;
                }
                ++number1;
            }
        }

        public void InfiniteWhile()
        {
            while (true)
            {
                Console.WriteLine("iteration");
            }
        }
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <cstdint>

namespace StatementsPorting {

class WhileStatements : public System::Object
{
    typedef WhileStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void While(int32_t max);
    void EnclosedWhile(int32_t max1, int32_t max2);
    void InfiniteWhile();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "WhileStatements.h"

#include <system/string.h>
#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2591991192u, ::StatementsPorting::WhileStatements, ThisTypeBaseTypesInfo);

void WhileStatements::While(int32_t max)
{
    int32_t number = 0;
    while (number < max)
    {
        System::Console::WriteLine(number);
        ++number;
    }
}

void WhileStatements::EnclosedWhile(int32_t max1, int32_t max2)
{
    int32_t number1 = 0;
    while (number1 < max1)
    {
        int32_t number2 = 0;
        while (number2 < max2)
        {
            System::Console::WriteLine(number1 + number2);
            ++number2;
        }
        ++number1;
    }
}

void WhileStatements::InfiniteWhile()
{
    while (true)
    {
        System::Console::WriteLine(u"iteration");
    }
}

} // namespace StatementsPorting

{{< /highlight >}}
