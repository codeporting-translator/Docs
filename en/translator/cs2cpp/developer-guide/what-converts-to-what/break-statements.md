---
date: "2023-03-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "BreakStatements"
linktitle: "BreakStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2023-03-09"
weight: "1"
---

This example demonstrates how break statement inside loop is translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class BreakStatements
    {
        public void BreakForeach(int[] values, int max)
        {
            foreach (int value in values)
            {
                Console.WriteLine(value);
                if (value > max)
                    break;
            }
        }

        public void BreakEnclosedForeach(int[][] values, int max)
        {
            foreach (int[] row in values)
            {
                if (row.Length == 0)
                    break;
                foreach (int value in row)
                {
                    Console.WriteLine(value);
                    if (value > max)
                        break;
                }
            }
        }

        public void BreakFor(int max)
        {
            for (int index = 0; index < max; ++index)
            {
                Console.WriteLine(index);
                if (index % 5 == 4)
                    break;
            }
        }

        public void BreakEnclosedFor(int max1, int max2)
        {
            for (int index1 = 0; index1 < max1; ++index1)
            {
                if (index1 % 14 == 11)
                    break;
                for (int index2 = 0; index2 < max2; ++index2)
                {
                    Console.WriteLine(index1 + index2);
                    if (index2 % 13 == 7)
                        break;
                }
            }
        }

        public void BreakWhile(int max)
        {
            int number = 0;
            while (number < max)
            {
                Console.WriteLine(number);
                if (number % 5 == 4)
                    break;
                ++number;
            }
        }

        public void BreakEnclosedWhile(int max1, int max2)
        {
            int number1 = 0;
            while (number1 < max1)
            {
                if (number1 % 14 == 11)
                    break;
                int number2 = 0;
                while (number2 < max2)
                {
                    Console.WriteLine(number1 + number2);
                    if (number2 % 5 == 4)
                        break;
                    ++number2;
                }
                ++number1;
            }
        }

        public void BreakDoWhile(int max)
        {
            int number = 0;
            do
            {
                Console.WriteLine(number);
                if (number % 5 == 4)
                    break;
                ++number;
            } while (number < max);
        }

        public void BreakEnclosedDoWhile(int max1, int max2)
        {
            int number1 = 0;
            do
            {
                if (number1 % 14 == 11)
                    break;
                int number2 = 0;
                do
                {
                    Console.WriteLine(number1 + number2);
                    if (number2 % 5 == 4)
                        break;
                    ++number2;
                } while (number2 < max2);
                ++number1;
            } while (number1 < max1);
        }
    }
}

{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/array.h>
#include <cstdint>

namespace StatementsPorting {

class BreakStatements : public System::Object
{
    typedef BreakStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void BreakForeach(System::ArrayPtr<int32_t> values, int32_t max);
    void BreakEnclosedForeach(System::ArrayPtr<System::ArrayPtr<int32_t>> values, int32_t max);
    void BreakFor(int32_t max);
    void BreakEnclosedFor(int32_t max1, int32_t max2);
    void BreakWhile(int32_t max);
    void BreakEnclosedWhile(int32_t max1, int32_t max2);
    void BreakDoWhile(int32_t max);
    void BreakEnclosedDoWhile(int32_t max1, int32_t max2);
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "BreakStatements.h"

#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(409202246u, ::StatementsPorting::BreakStatements, ThisTypeBaseTypesInfo);

void BreakStatements::BreakForeach(System::ArrayPtr<int32_t> values, int32_t max)
{
    for (int32_t value : values)
    {
        System::Console::WriteLine(value);
        if (value > max)
        {
            break;
        }
    }
    
}

void BreakStatements::BreakEnclosedForeach(System::ArrayPtr<System::ArrayPtr<int32_t>> values, int32_t max)
{
    for (System::ArrayPtr<int32_t> row : values)
    {
        if (row->get_Length() == 0)
        {
            break;
        }
        
        {
            for (int32_t value : row)
            {
                System::Console::WriteLine(value);
                if (value > max)
                {
                    break;
                }
            }
            
        }
    }
    
}

void BreakStatements::BreakFor(int32_t max)
{
    for (int32_t index = 0; index < max; ++index)
    {
        System::Console::WriteLine(index);
        if (index % 5 == 4)
        {
            break;
        }
    }
}

void BreakStatements::BreakEnclosedFor(int32_t max1, int32_t max2)
{
    for (int32_t index1 = 0; index1 < max1; ++index1)
    {
        if (index1 % 14 == 11)
        {
            break;
        }
        for (int32_t index2 = 0; index2 < max2; ++index2)
        {
            System::Console::WriteLine(index1 + index2);
            if (index2 % 13 == 7)
            {
                break;
            }
        }
    }
}

void BreakStatements::BreakWhile(int32_t max)
{
    int32_t number = 0;
    while (number < max)
    {
        System::Console::WriteLine(number);
        if (number % 5 == 4)
        {
            break;
        }
        ++number;
    }
}

void BreakStatements::BreakEnclosedWhile(int32_t max1, int32_t max2)
{
    int32_t number1 = 0;
    while (number1 < max1)
    {
        if (number1 % 14 == 11)
        {
            break;
        }
        int32_t number2 = 0;
        while (number2 < max2)
        {
            System::Console::WriteLine(number1 + number2);
            if (number2 % 5 == 4)
            {
                break;
            }
            ++number2;
        }
        ++number1;
    }
}

void BreakStatements::BreakDoWhile(int32_t max)
{
    int32_t number = 0;
    do
    {
        System::Console::WriteLine(number);
        if (number % 5 == 4)
        {
            break;
        }
        ++number;
    } while (number < max);
}

void BreakStatements::BreakEnclosedDoWhile(int32_t max1, int32_t max2)
{
    int32_t number1 = 0;
    do
    {
        if (number1 % 14 == 11)
        {
            break;
        }
        int32_t number2 = 0;
        do
        {
            System::Console::WriteLine(number1 + number2);
            if (number2 % 5 == 4)
            {
                break;
            }
            ++number2;
        } while (number2 < max2);
        ++number1;
    } while (number1 < max1);
}

} // namespace StatementsPorting

{{< /highlight >}}
