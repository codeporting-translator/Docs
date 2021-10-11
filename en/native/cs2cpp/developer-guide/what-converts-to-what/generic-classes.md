---
date: "2021-10-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "GenericClasses"
linktitle: "GenericClasses"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-10-09"
weight: "1"
---

This example demonstrates how generic classes are ported to C++. They become C++ template classes which are inherited from System::Object and have RTTI declared.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace TypesPorting
{
    public class GenericClass<TInner>
    {
    }

    public class GenericClassWithTypeConstraint<TInner> where TInner : ICloneable
    {
    }

    public class GenericClassWithClassConstraint<TInner> where TInner : class
    {
    }

    public class GenericClassWithStructConstraint<TInner> where TInner : struct
    {
    }

    public class GenericClassWithNewConstraint<TInner> where TInner : new()
    {
    }

    public class GenericClassWithSeveralConstraints<TInner> where TInner : class, ICloneable, new()
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
class GenericClass : public System::Object
{
    typedef GenericClass<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericClass;
    
public:

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
class GenericClassWithTypeConstraint : public System::Object
{
    typedef System::ICloneable BaseT_ICloneable;
    assert_is_base_of(BaseT_ICloneable, TInner);
    
    typedef GenericClassWithTypeConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericClassWithTypeConstraint;
    
public:

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
class GenericClassWithClassConstraint : public System::Object
{
    assert_is_cs_class(TInner);
    
    typedef GenericClassWithClassConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericClassWithClassConstraint;
    
public:

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
class GenericClassWithStructConstraint : public System::Object
{
    assert_is_cs_struct(TInner);
    
    typedef GenericClassWithStructConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericClassWithStructConstraint;
    
public:

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
class GenericClassWithNewConstraint : public System::Object
{
    assert_is_constructable(TInner);
    
    typedef GenericClassWithNewConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericClassWithNewConstraint;
    
public:

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
class GenericClassWithSeveralConstraints : public System::Object
{
    assert_is_cs_class(TInner);
    typedef System::ICloneable BaseT_ICloneable;
    assert_is_base_of(BaseT_ICloneable, TInner);
    assert_is_constructable(TInner);
    
    typedef GenericClassWithSeveralConstraints<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
    template<typename FT0> friend class GenericClassWithSeveralConstraints;
    
public:

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
