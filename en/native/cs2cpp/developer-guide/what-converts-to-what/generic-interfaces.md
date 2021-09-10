---
date: "2021-09-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "GenericInterfaces"
linktitle: "GenericInterfaces"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-09-10"
weight: "1"
---

This example demonstrates how generic interfaces are ported to C++. They become C++ template classes which are inherited from System::Object and have RTTI declared.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace TypesPorting
{
    public interface IGenericInterface<TInner>
    {
    }

    public interface IGenericInterfaceWithTypeConstraint<TInner> where TInner : ICloneable
    {
    }

    public interface IGenericInterfaceWithClassConstraint<TInner> where TInner : class
    {
    }

    public interface IGenericInterfaceWithStructConstraint<TInner> where TInner : struct
    {
    }

    public interface IGenericInterfaceWithNewConstraint<TInner> where TInner : new()
    {
    }

    public interface IGenericInterfaceWithSeveralConstraints<TInner> where TInner : class, ICloneable, new()
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
#include <system/constraints.h>

namespace TypesPorting {

template<typename TInner>
class IGenericInterface : public System::Object
{
    typedef IGenericInterface<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
};

template<typename TInner>
class IGenericInterfaceWithTypeConstraint : public System::Object
{
    typedef System::ICloneable BaseT_ICloneable;
    assert_is_base_of(BaseT_ICloneable, TInner);
    
    typedef IGenericInterfaceWithTypeConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
};

template<typename TInner>
class IGenericInterfaceWithClassConstraint : public System::Object
{
    assert_is_cs_class(TInner);
    
    typedef IGenericInterfaceWithClassConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
};

template<typename TInner>
class IGenericInterfaceWithStructConstraint : public System::Object
{
    assert_is_cs_struct(TInner);
    
    typedef IGenericInterfaceWithStructConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
};

template<typename TInner>
class IGenericInterfaceWithNewConstraint : public System::Object
{
    assert_is_constructable(TInner);
    
    typedef IGenericInterfaceWithNewConstraint<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
};

template<typename TInner>
class IGenericInterfaceWithSeveralConstraints : public System::Object
{
    assert_is_cs_class(TInner);
    typedef System::ICloneable BaseT_ICloneable;
    assert_is_base_of(BaseT_ICloneable, TInner);
    assert_is_constructable(TInner);
    
    typedef IGenericInterfaceWithSeveralConstraints<TInner> ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
    
};

} // namespace TypesPorting



{{< /highlight >}}
