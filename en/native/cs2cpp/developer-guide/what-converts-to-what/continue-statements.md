---
date: "2021-12-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ContinueStatements"
linktitle: "ContinueStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-12-09"
weight: "1"
---

This example demonstrates how continue statement inside loop is ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class ContinueStatements
    {
        public void ContinueForeach(int[] values, int max)
        {
            foreach (int value in values)
            {
                if (value > max)
                    continue;
                Console.WriteLine(value);
            }
        }

        public void ContinueEnclosedForeach(int[][] values, int max)
        {
            foreach (int[] row in values)
            {
                if (row.Length == 0)
                    continue;
                foreach (int value in row)
                {
                    if (value > max)
                        continue;
                    Console.WriteLine(value);
                }
            }
        }

        public void ContinueFor(int max)
        {
            for (int index = 0; index < max; ++index)
            {
                if (index % 5 == 4)
                    continue;
                Console.WriteLine(index);
            }
        }

        public void ContinueEnclosedFor(int max1, int max2)
        {
            for (int index1 = 0; index1 < max1; ++index1)
            {
                if (index1 % 14 == 11)
                    continue;
                for (int index2 = 0; index2 < max2; ++index2)
                {
                    if (index2 % 13 == 7)
                        continue;
                    Console.WriteLine(index1 + index2);
                }
            }
        }

        public void ContinueWhile(int max)
        {
            int number = 0;
            while (number < max)
            {
                if (number % 5 == 4)
                    continue;
                Console.WriteLine(number);
                ++number;
            }
        }

        public void ContinueEnclosedWhile(int max1, int max2)
        {
            int number1 = 0;
            while (number1 < max1)
            {
                if (number1 % 14 == 11)
                    continue;
                int number2 = 0;
                while (number2 < max2)
                {
                    if (number2 % 5 == 4)
                        continue;
                    Console.WriteLine(number1 + number2);
                    ++number2;
                }
                ++number1;
            }
        }

        public void ContinueDoWhile(int max)
        {
            int number = 0;
            do
            {
                if (number % 5 == 4)
                    continue;
                Console.WriteLine(number);
                ++number;
            } while (number < max);
        }

        public void ContinueEnclosedDoWhile(int max1, int max2)
        {
            int number1 = 0;
            do
            {
                if (number1 % 14 == 11)
                    continue;
                int number2 = 0;
                do
                {
                    if (number2 % 5 == 4)
                        continue;
                    Console.WriteLine(number1 + number2);
                    ++number2;
                } while (number2 < max2);
                ++number1;
            } while (number1 < max1);
        }
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/array.h>
#include <cstdint>

namespace StatementsPorting {

class ContinueStatements : public System::Object
{
    typedef ContinueStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void ContinueForeach(System::ArrayPtr<int32_t> values, int32_t max);
    void ContinueEnclosedForeach(System::ArrayPtr<System::ArrayPtr<int32_t>> values, int32_t max);
    void ContinueFor(int32_t max);
    void ContinueEnclosedFor(int32_t max1, int32_t max2);
    void ContinueWhile(int32_t max);
    void ContinueEnclosedWhile(int32_t max1, int32_t max2);
    void ContinueDoWhile(int32_t max);
    void ContinueEnclosedDoWhile(int32_t max1, int32_t max2);
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ContinueStatements.h"

#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2122136424u, ::StatementsPorting::ContinueStatements, ThisTypeBaseTypesInfo);

void ContinueStatements::ContinueForeach(System::ArrayPtr<int32_t> values, int32_t max)
{
    for (int32_t value : values)
    {
        if (value > max)
        {
            continue;
        }
        System::Console::WriteLine(value);
    }
    
}

void ContinueStatements::ContinueEnclosedForeach(System::ArrayPtr<System::ArrayPtr<int32_t>> values, int32_t max)
{
    for (System::ArrayPtr<int32_t> row : values)
    {
        if (row->get_Length() == 0)
        {
            continue;
        }
        
        {
            for (int32_t value : row)
            {
                if (value > max)
                {
                    continue;
                }
                System::Console::WriteLine(value);
            }
            
        }
    }
    
}

void ContinueStatements::ContinueFor(int32_t max)
{
    for (int32_t index = 0; index < max; ++index)
    {
        if (index % 5 == 4)
        {
            continue;
        }
        System::Console::WriteLine(index);
    }
}

void ContinueStatements::ContinueEnclosedFor(int32_t max1, int32_t max2)
{
    for (int32_t index1 = 0; index1 < max1; ++index1)
    {
        if (index1 % 14 == 11)
        {
            continue;
        }
        for (int32_t index2 = 0; index2 < max2; ++index2)
        {
            if (index2 % 13 == 7)
            {
                continue;
            }
            System::Console::WriteLine(index1 + index2);
        }
    }
}

void ContinueStatements::ContinueWhile(int32_t max)
{
    int32_t number = 0;
    while (number < max)
    {
        if (number % 5 == 4)
        {
            continue;
        }
        System::Console::WriteLine(number);
        ++number;
    }
}

void ContinueStatements::ContinueEnclosedWhile(int32_t max1, int32_t max2)
{
    int32_t number1 = 0;
    while (number1 < max1)
    {
        if (number1 % 14 == 11)
        {
            continue;
        }
        int32_t number2 = 0;
        while (number2 < max2)
        {
            if (number2 % 5 == 4)
            {
                continue;
            }
            System::Console::WriteLine(number1 + number2);
            ++number2;
        }
        ++number1;
    }
}

void ContinueStatements::ContinueDoWhile(int32_t max)
{
    int32_t number = 0;
    do
    {
        if (number % 5 == 4)
        {
            continue;
        }
        System::Console::WriteLine(number);
        ++number;
    } while (number < max);
}

void ContinueStatements::ContinueEnclosedDoWhile(int32_t max1, int32_t max2)
{
    int32_t number1 = 0;
    do
    {
        if (number1 % 14 == 11)
        {
            continue;
        }
        int32_t number2 = 0;
        do
        {
            if (number2 % 5 == 4)
            {
                continue;
            }
            System::Console::WriteLine(number1 + number2);
            ++number2;
        } while (number2 < max2);
        ++number1;
    } while (number1 < max1);
}

} // namespace StatementsPorting

{{< /highlight >}}
