---
date: "2024-05-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "DoWhileStatements"
linktitle: "DoWhileStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2024-05-10"
weight: "1"
---

This example demonstrates how do-while statement is translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class DoWhileStatements
    {
        public void DoWhile(int max)
        {
            int number = 0;
            do
            {
                Console.WriteLine(number);
                ++number;
            } while (number < max);
        }

        public void EnclosedDoWhile(int max1, int max2)
        {
            int number1 = 0;
            do
            {
                int number2 = 0;
                do
                {
                    Console.WriteLine(number1 + number2);
                    ++number2;
                } while (number2 < max2);
                ++number1;
            } while (number1 < max1);
        }

        public void InfiniteDoWhile()
        {
            do
            {
                Console.WriteLine("iteration");
            } while (true);
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

class DoWhileStatements : public System::Object
{
    typedef DoWhileStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void DoWhile(int32_t max);
    void EnclosedDoWhile(int32_t max1, int32_t max2);
    void InfiniteDoWhile();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "DoWhileStatements.h"

#include <system/string.h>
#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(1608234029u, ::StatementsPorting::DoWhileStatements, ThisTypeBaseTypesInfo);

void DoWhileStatements::DoWhile(int32_t max)
{
    int32_t number = 0;
    do
    {
        System::Console::WriteLine(number);
        ++number;
    } while (number < max);
}

void DoWhileStatements::EnclosedDoWhile(int32_t max1, int32_t max2)
{
    int32_t number1 = 0;
    do
    {
        int32_t number2 = 0;
        do
        {
            System::Console::WriteLine(number1 + number2);
            ++number2;
        } while (number2 < max2);
        ++number1;
    } while (number1 < max1);
}

void DoWhileStatements::InfiniteDoWhile()
{
    do
    {
        System::Console::WriteLine(u"iteration");
    } while (true);
}

} // namespace StatementsPorting

{{< /highlight >}}
