---
date: "2022-06-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ForeachStatementsStd"
linktitle: "ForeachStatementsStd"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-06-09"
weight: "1"
---

This example demonstrates standard way how foreach statement is ported to C++. It is translated to C++ while statement.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: -o foreach_as_range_based_for_loop=false.

## Source C# Code ##

{{< highlight cs >}}
using System;
using System.Collections;
using System.Collections.Generic;
using CodePorting.Translator.Cs2Cpp;

namespace StatementsPorting
{
    public struct TheStruct
    {
        public override string ToString()
        {
            return "The struct";
        }
    }

    public class TheClass
    {
        public override string ToString()
        {
            return "The class";
        }
    }

    public class ForeachStatementsStd
    {
        public void ForeachValueType_SingleStatement()
        {
            var structList = new List<TheStruct>() { new TheStruct() };
            foreach (var s in structList)
                Console.WriteLine(s.ToString());
        }

        public void ForeachReferenceType_BlockStatement()
        {
            IList<TheClass> classList = new List<TheClass>() { new TheClass() };
            foreach (var c in classList)
            {
                Console.WriteLine(c);
            }
        }
    }
}

{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/string.h>

namespace StatementsPorting {

class TheStruct : public System::Object
{
    typedef TheStruct ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    TheStruct();
    
    System::String ToString() const override;
    
private:

    System::String ToString_NonConst();
    
};

class TheClass : public System::Object
{
    typedef TheClass ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    System::String ToString() const override;
    
private:

    System::String ToString_NonConst();
    
};

class ForeachStatementsStd : public System::Object
{
    typedef ForeachStatementsStd ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void ForeachValueType_SingleStatement();
    void ForeachReferenceType_BlockStatement();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ForeachStatementsStd.h"

#include <system/object_ext.h>
#include <system/console.h>
#include <system/collections/list.h>
#include <system/collections/ilist.h>
#include <system/collections/ienumerator.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2692051849u, ::StatementsPorting::TheStruct, ThisTypeBaseTypesInfo);

System::String TheStruct::ToString_NonConst()
{
    return u"The struct";
}

TheStruct::TheStruct()
{
}

System::String TheStruct::ToString() const
{
    using Type = typename std::remove_const<typename std::remove_reference<decltype(*this)>::type>::type;
    using TypePtr = typename std::add_pointer<Type>::type;
    return const_cast<TypePtr>(this)->ToString_NonConst();
}

RTTI_INFO_IMPL_HASH(548796836u, ::StatementsPorting::TheClass, ThisTypeBaseTypesInfo);

System::String TheClass::ToString_NonConst()
{
    return u"The class";
}

System::String TheClass::ToString() const
{
    using Type = typename std::remove_const<typename std::remove_reference<decltype(*this)>::type>::type;
    using TypePtr = typename std::add_pointer<Type>::type;
    return const_cast<TypePtr>(this)->ToString_NonConst();
}

RTTI_INFO_IMPL_HASH(3070297266u, ::StatementsPorting::ForeachStatementsStd, ThisTypeBaseTypesInfo);

void ForeachStatementsStd::ForeachValueType_SingleStatement()
{
    auto structList = [&]{ StatementsPorting::TheStruct init_0[] = {TheStruct()}; auto list_0 = System::MakeObject<System::Collections::Generic::List<TheStruct>>(); list_0->AddInitializer(1, init_0); return list_0; }();
    
    {
        auto s_enumerator = (structList)->GetEnumerator();
        while (s_enumerator->MoveNext())
        {
            auto&& s = s_enumerator->get_Current();
            
            System::Console::WriteLine(System::ObjectExt::ToString(s));
        }
    }
}

void ForeachStatementsStd::ForeachReferenceType_BlockStatement()
{
    System::SharedPtr<System::Collections::Generic::IList<System::SharedPtr<TheClass>>> classList = [&]{ System::SharedPtr<StatementsPorting::TheClass> init_1[] = {System::MakeObject<TheClass>()}; auto list_1 = System::MakeObject<System::Collections::Generic::List<System::SharedPtr<TheClass>>>(); list_1->AddInitializer(1, init_1); return list_1; }();
    
    {
        auto c_enumerator = (classList)->GetEnumerator();
        while (c_enumerator->MoveNext())
        {
            auto&& c = c_enumerator->get_Current();
            
            System::Console::WriteLine(c);
        }
    }
}

} // namespace StatementsPorting

{{< /highlight >}}
