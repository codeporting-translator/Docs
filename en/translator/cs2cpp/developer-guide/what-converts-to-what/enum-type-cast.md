---
date: "2023-07-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "EnumTypeCast"
linktitle: "EnumTypeCast"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2023-07-09"
weight: "1"
---

This example demonstrates how C# enum type cast expression is translated to C++.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace StatementsPorting
{
    public enum SomeEnum
    {
        Value1 = 1,
        Value3 = 3,
        Value13 = 13
    }

    public class EnumTypeCast
    {
        public void EnumToNumberCasts()
        {
            SomeEnum source = SomeEnum.Value13;
            sbyte dest1 = (sbyte) source;
            byte dest2 = (byte) source;
            short dest3 = (short) source;
            ushort dest4 = (ushort) source;
            int dest5 = (int) source;
            uint dest6 = (uint) source;
            long dest7 = (long) source;
            ulong dest8 = (ulong) source;
            float dest9 = (float) source;
            double dest10 = (double) source;
            char dest11 = (char) source;
        }

        public void NumberEnumToCasts()
        {
            sbyte source1 = -11;
            byte source2 = 13;
            short source3 = -333;
            ushort source4 = 666;
            int source5 = -333333;
            uint source6 = 666666;
            long source7 = -333333333;
            ulong source8 = 666666666;
            float source9 = 3.1415926f;
            double source10 = 3.1415926;
            char source11 = 'X';
            SomeEnum dest1 = (SomeEnum) source1;
            SomeEnum dest2 = (SomeEnum) source2;
            SomeEnum dest3 = (SomeEnum) source3;
            SomeEnum dest4 = (SomeEnum) source4;
            SomeEnum dest5 = (SomeEnum) source5;
            SomeEnum dest6 = (SomeEnum) source6;
            SomeEnum dest7 = (SomeEnum) source7;
            SomeEnum dest8 = (SomeEnum) source8;
            SomeEnum dest9 = (SomeEnum) source9;
            SomeEnum dest10 = (SomeEnum) source10;
            SomeEnum dest11 = (SomeEnum) source11;
        }
    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>

namespace StatementsPorting {

enum class SomeEnum
{
    Value1 = 1,
    Value3 = 3,
    Value13 = 13
};

class EnumTypeCast : public System::Object
{
    typedef EnumTypeCast ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void EnumToNumberCasts();
    void NumberEnumToCasts();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "EnumTypeCast.h"

#include <cstdint>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(1083267223u, ::StatementsPorting::EnumTypeCast, ThisTypeBaseTypesInfo);

void EnumTypeCast::EnumToNumberCasts()
{
    SomeEnum source = StatementsPorting::SomeEnum::Value13;
    int8_t dest1 = (int8_t)source;
    uint8_t dest2 = (uint8_t)source;
    int16_t dest3 = (int16_t)source;
    uint16_t dest4 = (uint16_t)source;
    int32_t dest5 = (int32_t)source;
    uint32_t dest6 = (uint32_t)source;
    int64_t dest7 = (int64_t)source;
    uint64_t dest8 = (uint64_t)source;
    float dest9 = (float)source;
    double dest10 = (double)source;
    char16_t dest11 = (char16_t)source;
}

void EnumTypeCast::NumberEnumToCasts()
{
    int8_t source1 = -11;
    uint8_t source2 = 13;
    int16_t source3 = -333;
    uint16_t source4 = 666;
    int32_t source5 = -333333;
    uint32_t source6 = 666666;
    int64_t source7 = -333333333;
    uint64_t source8 = 666666666;
    float source9 = 3.1415926f;
    double source10 = 3.1415926;
    char16_t source11 = u'X';
    SomeEnum dest1 = (SomeEnum)source1;
    SomeEnum dest2 = (SomeEnum)source2;
    SomeEnum dest3 = (SomeEnum)source3;
    SomeEnum dest4 = (SomeEnum)source4;
    SomeEnum dest5 = (SomeEnum)source5;
    SomeEnum dest6 = (SomeEnum)source6;
    SomeEnum dest7 = (SomeEnum)source7;
    SomeEnum dest8 = (SomeEnum)source8;
    SomeEnum dest9 = (SomeEnum)source9;
    SomeEnum dest10 = (SomeEnum)source10;
    SomeEnum dest11 = (SomeEnum)source11;
}

} // namespace StatementsPorting

{{< /highlight >}}
