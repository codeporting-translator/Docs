---
date: "2022-01-14"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "GenericStructs"
linktitle: "GenericStructs"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-01-14"
weight: "1"
---

This example demonstrates how C# generic structs are ported to C++. They become C++ template classes which are inherited from System::Object and have RTTI declared.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace TypesPorting
{
    public struct GenericStruct<TInner>
    {
    }

    public struct GenericStructWithTypeConstraint<TInner> where TInner : ICloneable
    {
    }

    public struct GenericStructWithClassConstraint<TInner> where TInner : class
    {
    }

    public struct GenericStructWithStructConstraint<TInner> where TInner : struct
    {
    }

    public struct GenericStructWithNewConstraint<TInner> where TInner : new()
    {
    }

    public struct GenericStructWithSeveralConstraints<TInner> where TInner : class, ICloneable, new()
    {
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <system/icloneable.h>
#include <system/details/pointer_collection_helpers.h>
#include <system/constraints.h>
#include <cstdint>

namespace TypesPorting {

template<typename TInner>
class GenericStruct : public System::Object
{
    typedef GenericStruct<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericStruct;
    
public:

    GenericStruct()
    {
    }
    
    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
                
        }
    }
    
};

template<typename TInner>
class GenericStructWithTypeConstraint : public System::Object
{
    typedef System::ICloneable BaseT_ICloneable;
    assert_is_base_of(BaseT_ICloneable, TInner);
    
    typedef GenericStructWithTypeConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericStructWithTypeConstraint;
    
public:

    GenericStructWithTypeConstraint()
    {
    }
    
    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
                
        }
    }
    
};

template<typename TInner>
class GenericStructWithClassConstraint : public System::Object
{
    assert_is_cs_class(TInner);
    
    typedef GenericStructWithClassConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericStructWithClassConstraint;
    
public:

    GenericStructWithClassConstraint()
    {
    }
    
    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
                
        }
    }
    
};

template<typename TInner>
class GenericStructWithStructConstraint : public System::Object
{
    assert_is_cs_struct(TInner);
    
    typedef GenericStructWithStructConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericStructWithStructConstraint;
    
public:

    GenericStructWithStructConstraint()
    {
    }
    
    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
                
        }
    }
    
};

template<typename TInner>
class GenericStructWithNewConstraint : public System::Object
{
    assert_is_constructable(TInner);
    
    typedef GenericStructWithNewConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericStructWithNewConstraint;
    
public:

    GenericStructWithNewConstraint()
    {
    }
    
    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
                
        }
    }
    
};

template<typename TInner>
class GenericStructWithSeveralConstraints : public System::Object
{
    assert_is_cs_class(TInner);
    typedef System::ICloneable BaseT_ICloneable;
    assert_is_base_of(BaseT_ICloneable, TInner);
    assert_is_constructable(TInner);
    
    typedef GenericStructWithSeveralConstraints<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericStructWithSeveralConstraints;
    
public:

    GenericStructWithSeveralConstraints()
    {
    }
    
    void SetTemplateWeakPtr(uint32_t argument) override
    {
        switch (argument)
        {
            case 0:
                break;
                
        }
    }
    
};

} // namespace TypesPorting



{{< /highlight >}}
