---
date: "2026-01-16"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassGenericMethods"
linktitle: "ClassGenericMethods"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2026-01-16"
weight: "1"
---

This example demonstrates how generic class methods are translated to C++. In C++ they become template methods.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace MembersPorting
{
    public class ClassGenericMethods
    {
        public void NongenericMethod()
        {
        }

        public void GenericMethod<T>(T value)
        {
        }

        public void GenericMethodWithTypeConstraint<T>(T value) where T : ICloneable
        {
        }

        public void GenericMethodWithClassConstraint<T>(T value) where T : class
        {
        }

        public void GenericMethodWithStructConstraint<T>(T value) where T : struct
        {
        }

        public void GenericMethodWithNewConstraint<T>(T value) where T : new()
        {
        }

        public void GenericMethodWithSeveralConstraint<T>(T value) where T : class, ICloneable, new()
        {
        }
    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <system/icloneable.h>
#include <system/constraints.h>

namespace MembersPorting {

class ClassGenericMethods : public System::Object
{
    typedef ClassGenericMethods ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void NongenericMethod();
    template <typename T>
    void GenericMethod(T value)
    {
        ASPOSE_UNUSED(value);
    }
    
    template <typename T>
    void GenericMethodWithTypeConstraint(T value)
    {
        typedef System::ICloneable BaseT_ICloneable;
        assert_is_base_of(BaseT_ICloneable, T);
        
        ASPOSE_UNUSED(value);
    }
    
    template <typename T>
    void GenericMethodWithClassConstraint(T value)
    {
        assert_is_cs_class(T);
        
        ASPOSE_UNUSED(value);
    }
    
    template <typename T>
    void GenericMethodWithStructConstraint(T value)
    {
        assert_is_cs_struct(T);
        
        ASPOSE_UNUSED(value);
    }
    
    template <typename T>
    void GenericMethodWithNewConstraint(T value)
    {
        assert_is_constructable(T);
        
        ASPOSE_UNUSED(value);
    }
    
    template <typename T>
    void GenericMethodWithSeveralConstraint(T value)
    {
        assert_is_cs_class(T);
        typedef System::ICloneable BaseT_ICloneable;
        assert_is_base_of(BaseT_ICloneable, T);
        assert_is_constructable(T);
        
        ASPOSE_UNUSED(value);
    }
    
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassGenericMethods.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(821730475u, ::MembersPorting::ClassGenericMethods, ThisTypeBaseTypesInfo);

void ClassGenericMethods::NongenericMethod()
{
}

} // namespace MembersPorting

{{< /highlight >}}
