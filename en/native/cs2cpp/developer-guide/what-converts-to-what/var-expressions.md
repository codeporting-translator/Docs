---
date: "2021-12-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "VarExpressions"
linktitle: "VarExpressions"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-12-09"
weight: "1"
---

This example demonstrates how var expression is ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace StatementsPorting
{
    public class SomeClass
    {
    }

    public class VarExpressions
    {
        public void Expressions()
        {
            var value1 = 666;
            var value2 = 666U;
            var value3 = 666L;
            var value4 = 666LU;
            var value5 = 1.2345f;
            var value6 = 1.2345;
            var value7 = 'X';
            var value8 = "iddqd";
            var value9 = new SomeClass();
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

class SomeClass : public System::Object
{
    typedef SomeClass ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
};

class VarExpressions : public System::Object
{
    typedef VarExpressions ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void Expressions();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "VarExpressions.h"

#include <system/string.h>
#include <cstdint>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(1646106215u, ::StatementsPorting::SomeClass, ThisTypeBaseTypesInfo);

RTTI_INFO_IMPL_HASH(2520391057u, ::StatementsPorting::VarExpressions, ThisTypeBaseTypesInfo);

void VarExpressions::Expressions()
{
    int32_t value1 = 666;
    uint32_t value2 = 666U;
    int64_t value3 = INT64_C(666);
    uint64_t value4 = UINT64_C(666);
    float value5 = 1.2345f;
    double value6 = 1.2345;
    char16_t value7 = u'X';
    System::String value8 = u"iddqd";
    auto value9 = System::MakeObject<SomeClass>();
}

} // namespace StatementsPorting

{{< /highlight >}}
