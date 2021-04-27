---
date: "2021-04-27"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: false
title: "ForStatements"
linktitle: "ForStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-04-27"
weight: "1"
---

This example demonstrates how for statement is ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class ForStatements
    {
        public void For(int count)
        {
            for (int index = 0; index < count; ++index)
            {
                Console.WriteLine(index);
            }
        }

        public void ForWithSeveralCounter(int count1, int count2)
        {
            for (int index1 = 0, index2 = 0; index1 < count1 && index2 < count2; ++index1, ++index2)
            {
                Console.WriteLine(index1 + index2);
            }
        }

        public void EnclosedFor(int count1, int count2)
        {
            for (int index1 = 0; index1 < count1; ++index1)
            for (int index2 = 0; index2 < count2; ++index2)
            {
                Console.WriteLine(index1 + index2);
            }
        }

        public void CustomizedFor(int max)
        {
            int prev = 1;
            int current = 1;
            for (; current <= max;)
            {
                Console.WriteLine(current);
                int next = current + prev;
                prev = current;
                current = next;
            }
        }

        public void InfiniteFor()
        {
            for (;;)
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

class ForStatements : public System::Object
{
    typedef ForStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void For(int32_t count);
    void ForWithSeveralCounter(int32_t count1, int32_t count2);
    void EnclosedFor(int32_t count1, int32_t count2);
    void CustomizedFor(int32_t max);
    void InfiniteFor();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ForStatements.h"

#include <system/string.h>
#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(772585040u, ::StatementsPorting::ForStatements, ThisTypeBaseTypesInfo);

void ForStatements::For(int32_t count)
{
    for (int32_t index = 0; index < count; ++index)
    {
        System::Console::WriteLine(index);
    }
}

void ForStatements::ForWithSeveralCounter(int32_t count1, int32_t count2)
{
    for (int32_t index1 = 0, index2 = 0; index1 < count1 && index2 < count2; ++index1, ++index2)
    {
        System::Console::WriteLine(index1 + index2);
    }
}

void ForStatements::EnclosedFor(int32_t count1, int32_t count2)
{
    for (int32_t index1 = 0; index1 < count1; ++index1)
    {
        for (int32_t index2 = 0; index2 < count2; ++index2)
        {
            System::Console::WriteLine(index1 + index2);
        }
    }
}

void ForStatements::CustomizedFor(int32_t max)
{
    int32_t prev = 1;
    int32_t current = 1;
    for (; current <= max; )
    {
        System::Console::WriteLine(current);
        int32_t next = current + prev;
        prev = current;
        current = next;
    }
}

void ForStatements::InfiniteFor()
{
    for (; ; )
    {
        System::Console::WriteLine(u"iteration");
    }
}

} // namespace StatementsPorting

{{< /highlight >}}
