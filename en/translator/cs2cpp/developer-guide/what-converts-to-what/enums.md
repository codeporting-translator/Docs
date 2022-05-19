---
date: "2022-05-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "Enums"
linktitle: "Enums"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-05-09"
weight: "1"
---

This example demonstrates how C# enums are translated to C++. Flags enums have additional declarations for supporting logical operations.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace TypesPorting
{
    public enum SimpleEnum
    {
        Value1 = 1,
        Value2,
        Value3 = 100,
        Value4 = -1,
        Value5 = -Value3,
        Value6 = -5,
        Value7 = 7
    }

    public enum EnumWithType : uint
    {
        Value1 = 1,
        Value2,
        Value3 = 100,
        Value4 = SimpleEnum.Value7 
    }

    [Flags]
    public enum SimpleFlags
    {
        Value1 = 1,
        Value2 = 2,
        Value3 = 4,
        Value4 = 8
    }

    [Flags]
    public enum FlagsWithType : uint
    {
        Value1 = 1,
        Value2 = 2,
        Value3 = 4,
        Value4 = 8
    }

}

{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/enum_helpers.h>
#include <cstdint>

namespace TypesPorting {

enum class SimpleEnum
{
    Value1 = 1,
    Value2,
    Value3 = 100,
    Value4 = -1,
    Value5 = static_cast<int32_t>(-Value3),
    Value6 = -5,
    Value7 = 7
};

enum class EnumWithType : uint32_t
{
    Value1 = 1,
    Value2,
    Value3 = 100,
    Value4 = static_cast<uint32_t>(TypesPorting::SimpleEnum::Value7)
};

enum class SimpleFlags
{
    Value1 = 1,
    Value2 = 2,
    Value3 = 4,
    Value4 = 8
};

DECLARE_ENUM_OPERATORS(TypesPorting::SimpleFlags);
DECLARE_USING_GLOBAL_OPERATORS

enum class FlagsWithType : uint32_t
{
    Value1 = 1,
    Value2 = 2,
    Value3 = 4,
    Value4 = 8
};

DECLARE_ENUM_OPERATORS(TypesPorting::FlagsWithType);
DECLARE_USING_GLOBAL_OPERATORS

} // namespace TypesPorting

DECLARE_USING_ENUM_OPERATORS(TypesPorting);




{{< /highlight >}}
