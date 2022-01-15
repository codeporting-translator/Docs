---
date: "2022-01-14"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "StandardTypeCast"
linktitle: "StandardTypeCast"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-01-14"
weight: "1"
---

This example demonstrates how C# standard type cast expression is ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace StatementsPorting
{
    public class StandardTypeCast
    {
        public void SByteTypeCasts()
        {
            sbyte source = -11;
            // implicit casts
            short dest1 = source;
            int dest2 = source;
            long dest3 = source;
            float dest4 = source;
            double dest5 = source;
            // explicit casts
            byte dest6 = (byte) source;
            ushort dest7 = (ushort) source;
            uint dest8 = (uint) source;
            ulong dest9 = (ulong) source;
            char dest10 = (char) source;
        }

        public void ByteTypeCasts()
        {
            byte source = 13;
            // implicit casts
            short dest1 = source;
            ushort dest2 = source;
            int dest3 = source;
            uint dest4 = source;
            long dest5 = source;
            ulong dest6 = source;
            float dest7 = source;
            double dest8 = source;
            // explicit casts
            sbyte dest9 = (sbyte) source;
            char dest10 = (char) source;
        }

        public void ShortTypeCasts()
        {
            short source = -333;
            // implicit casts
            int dest1 = source;
            long dest2 = source;
            float dest3 = source;
            double dest4 = source;
            // explicit casts
            sbyte dest5 = (sbyte) source;
            byte dest6 = (byte) source;
            ushort dest7 = (ushort) source;
            uint dest8 = (uint) source;
            ulong dest9 = (ulong) source;
            char dest10 = (char) source;
        }

        public void UShortTypeCasts()
        {
            ushort source = 666;
            // implicit casts
            int dest1 = source;
            uint dest2 = source;
            long dest3 = source;
            ulong dest4 = source;
            float dest5 = source;
            double dest6 = source;
            // explicit casts
            sbyte dest7 = (sbyte) source;
            byte dest8 = (byte) source;
            short dest9 = (short) source;
            char dest10 = (char) source;
        }

        public void IntTypeCasts()
        {
            int source = -333333;
            // implicit casts
            long dest1 = source;
            float dest2 = source;
            double dest3 = source;
            // explicit casts
            sbyte dest4 = (sbyte) source;
            byte dest5 = (byte) source;
            short dest6 = (short) source;
            ushort dest7 = (ushort) source;
            uint dest8 = (uint) source;
            ulong dest9 = (ulong) source;
            char dest10 = (char) source;
        }

        public void UIntTypeCasts()
        {
            uint source = 666666;
            // implicit casts;
            long dest1 = source;
            ulong dest2 = source;
            float dest3 = source;
            double dest4 = source;
            // explicit casts
            sbyte dest5 = (sbyte) source;
            byte dest6 = (byte) source;
            short dest7 = (short) source;
            ushort dest8 = (ushort) source;
            int dest9 = (int) source;
            char dest10 = (char)source;
        }

        public void LongTypeCasts()
        {
            long source = -333333333;
            // implicit casts
            float dest1 = source;
            double dest2 = source;
            // explicit casts
            sbyte dest3 = (sbyte) source;
            byte dest4 = (byte) source;
            short dest5 = (short) source;
            ushort dest6 = (ushort) source;
            int dest7 = (int) source;
            uint dest8 = (uint) source;
            ulong dest9 = (ulong) source;
            char dest10 = (char) source;
        }

        public void ULongTypeCasts()
        {
            ulong source = 666666666;
            // implicit casts;
            float dest1 = source;
            double dest2 = source;
            // explicit casts
            sbyte dest3 = (sbyte) source;
            byte dest4 = (byte) source;
            short dest5 = (short) source;
            ushort dest6 = (ushort) source;
            int dest7 = (int) source;
            uint dest8 = (uint) source;
            long dest9 = (long) source;
            char dest10 = (char)source;
        }

        public void FloatTypeCasts()
        {
            float source = 3.1415926f;
            // implicit casts
            double dest1 = source;
            // explicit casts
            sbyte dest2 = (sbyte) source;
            byte dest3 = (byte) source;
            short dest4 = (short) source;
            ushort dest5 = (ushort) source;
            int dest6 = (int) source;
            uint dest7 = (uint) source;
            long dest8 = (long) source;
            ulong dest9 = (ulong) source;
            char dest10 = (char) source;
        }

        public void DoubleTypeCasts()
        {
            double source = 3.1415926;
            // explicit casts
            sbyte dest1 = (sbyte) source;
            byte dest2 = (byte) source;
            short dest3 = (short) source;
            ushort dest4 = (ushort) source;
            int dest5 = (int) source;
            uint dest6 = (uint) source;
            long dest7 = (long) source;
            ulong dest8 = (ulong) source;
            float dest9 = (float) source;
            char dest10 = (char)source;
        }

        public void CharTypeCasts()
        {
            char source = 'A';
            // implicit casts
            ushort dest1 = source;
            int dest2 = source;
            uint dest3 = source;
            long dest4 = source;
            ulong dest5 = source;
            float dest6 = source;
            double dest7 = source;
            // explicit casts
            sbyte dest8 = (sbyte) source;
            byte dest9 = (byte) source;
            short dest10 = (short) source;
        }
        
        public void OperandCasts()
        {
            double maxDays1 = long.MaxValue / (TimeSpan.TicksPerMillisecond / 1000.0 / 60.0 / 60.0 / 24.0);
            double maxDays2 = 9223372036854775807 / (TimeSpan.TicksPerMillisecond / 1000.0 / 60.0 / 60.0 / 24.0);
            double maxDays3 = 9223372036854 / (TimeSpan.TicksPerMillisecond / 1000.0 / 60.0 / 60.0 / 24.0);
        }
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>

namespace StatementsPorting {

class StandardTypeCast : public System::Object
{
    typedef StandardTypeCast ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void SByteTypeCasts();
    void ByteTypeCasts();
    void ShortTypeCasts();
    void UShortTypeCasts();
    void IntTypeCasts();
    void UIntTypeCasts();
    void LongTypeCasts();
    void ULongTypeCasts();
    void FloatTypeCasts();
    void DoubleTypeCasts();
    void CharTypeCasts();
    void OperandCasts();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "StandardTypeCast.h"

#include <system/timespan.h>
#include <system/primitive_types.h>
#include <cstdint>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2854543123u, ::StatementsPorting::StandardTypeCast, ThisTypeBaseTypesInfo);

void StandardTypeCast::SByteTypeCasts()
{
    int8_t source = -11;
    // implicit casts
    int16_t dest1 = source;
    int32_t dest2 = source;
    int64_t dest3 = source;
    float dest4 = source;
    double dest5 = source;
    // explicit casts
    uint8_t dest6 = (uint8_t)source;
    uint16_t dest7 = (uint16_t)source;
    uint32_t dest8 = (uint32_t)source;
    uint64_t dest9 = (uint64_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::ByteTypeCasts()
{
    uint8_t source = 13;
    // implicit casts
    int16_t dest1 = source;
    uint16_t dest2 = source;
    int32_t dest3 = source;
    uint32_t dest4 = source;
    int64_t dest5 = source;
    uint64_t dest6 = source;
    float dest7 = source;
    double dest8 = source;
    // explicit casts
    int8_t dest9 = (int8_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::ShortTypeCasts()
{
    int16_t source = static_cast<int16_t>(-333);
    // implicit casts
    int32_t dest1 = source;
    int64_t dest2 = source;
    float dest3 = source;
    double dest4 = source;
    // explicit casts
    int8_t dest5 = (int8_t)source;
    uint8_t dest6 = (uint8_t)source;
    uint16_t dest7 = (uint16_t)source;
    uint32_t dest8 = (uint32_t)source;
    uint64_t dest9 = (uint64_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::UShortTypeCasts()
{
    uint16_t source = static_cast<uint16_t>(666);
    // implicit casts
    int32_t dest1 = source;
    uint32_t dest2 = source;
    int64_t dest3 = source;
    uint64_t dest4 = source;
    float dest5 = source;
    double dest6 = source;
    // explicit casts
    int8_t dest7 = (int8_t)source;
    uint8_t dest8 = (uint8_t)source;
    int16_t dest9 = (int16_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::IntTypeCasts()
{
    int32_t source = -333333;
    // implicit casts
    int64_t dest1 = source;
    float dest2 = static_cast<float>(source);
    double dest3 = source;
    // explicit casts
    int8_t dest4 = (int8_t)source;
    uint8_t dest5 = (uint8_t)source;
    int16_t dest6 = (int16_t)source;
    uint16_t dest7 = (uint16_t)source;
    uint32_t dest8 = (uint32_t)source;
    uint64_t dest9 = (uint64_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::UIntTypeCasts()
{
    uint32_t source = 666666;
    // implicit casts;
    int64_t dest1 = source;
    uint64_t dest2 = source;
    float dest3 = static_cast<float>(source);
    double dest4 = source;
    // explicit casts
    int8_t dest5 = (int8_t)source;
    uint8_t dest6 = (uint8_t)source;
    int16_t dest7 = (int16_t)source;
    uint16_t dest8 = (uint16_t)source;
    int32_t dest9 = (int32_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::LongTypeCasts()
{
    int64_t source = -333333333;
    // implicit casts
    float dest1 = static_cast<float>(source);
    double dest2 = static_cast<double>(source);
    // explicit casts
    int8_t dest3 = (int8_t)source;
    uint8_t dest4 = (uint8_t)source;
    int16_t dest5 = (int16_t)source;
    uint16_t dest6 = (uint16_t)source;
    int32_t dest7 = (int32_t)source;
    uint32_t dest8 = (uint32_t)source;
    uint64_t dest9 = (uint64_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::ULongTypeCasts()
{
    uint64_t source = 666666666;
    // implicit casts;
    float dest1 = static_cast<float>(source);
    double dest2 = static_cast<double>(source);
    // explicit casts
    int8_t dest3 = (int8_t)source;
    uint8_t dest4 = (uint8_t)source;
    int16_t dest5 = (int16_t)source;
    uint16_t dest6 = (uint16_t)source;
    int32_t dest7 = (int32_t)source;
    uint32_t dest8 = (uint32_t)source;
    int64_t dest9 = (int64_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::FloatTypeCasts()
{
    float source = 3.1415926f;
    // implicit casts
    double dest1 = source;
    // explicit casts
    int8_t dest2 = (int8_t)source;
    uint8_t dest3 = (uint8_t)source;
    int16_t dest4 = (int16_t)source;
    uint16_t dest5 = (uint16_t)source;
    int32_t dest6 = (int32_t)source;
    uint32_t dest7 = (uint32_t)source;
    int64_t dest8 = (int64_t)source;
    uint64_t dest9 = (uint64_t)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::DoubleTypeCasts()
{
    double source = 3.1415926;
    // explicit casts
    int8_t dest1 = (int8_t)source;
    uint8_t dest2 = (uint8_t)source;
    int16_t dest3 = (int16_t)source;
    uint16_t dest4 = (uint16_t)source;
    int32_t dest5 = (int32_t)source;
    uint32_t dest6 = (uint32_t)source;
    int64_t dest7 = (int64_t)source;
    uint64_t dest8 = (uint64_t)source;
    float dest9 = (float)source;
    char16_t dest10 = (char16_t)source;
}

void StandardTypeCast::CharTypeCasts()
{
    char16_t source = u'A';
    // implicit casts
    uint16_t dest1 = source;
    int32_t dest2 = source;
    uint32_t dest3 = source;
    int64_t dest4 = source;
    uint64_t dest5 = source;
    float dest6 = source;
    double dest7 = source;
    // explicit casts
    int8_t dest8 = (int8_t)source;
    uint8_t dest9 = (uint8_t)source;
    int16_t dest10 = (int16_t)source;
}

void StandardTypeCast::OperandCasts()
{
    double maxDays1 = static_cast<double>(std::numeric_limits<int64_t>::max()) / (System::TimeSpan::TicksPerMillisecond / 1000.0 / 60.0 / 60.0 / 24.0);
    double maxDays2 = static_cast<double>(9223372036854775807) / (System::TimeSpan::TicksPerMillisecond / 1000.0 / 60.0 / 60.0 / 24.0);
    double maxDays3 = 9223372036854 / (System::TimeSpan::TicksPerMillisecond / 1000.0 / 60.0 / 60.0 / 24.0);
}

} // namespace StatementsPorting

{{< /highlight >}}
