---
date: "2022-03-04"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "AbstractClasses"
linktitle: "AbstractClasses"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-03-04"
weight: "1"
---

This example demonstrates how abstract classes are ported to C++. They are declared using macro ABSTRACT which expands into __declspec(novtable) for Microsoft VC++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public abstract class AbstractClassWithoutMethods
    {
    }

    public abstract class AbstractClassWithMethods
    {
        public abstract void SomeAbstractMethod();
        public virtual void SomeVirtualMethod()
        {
        }
        public void SomeMethod()
        {
        }
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>

namespace TypesPorting {

class AbstractClassWithoutMethods : public System::Object
{
    typedef AbstractClassWithoutMethods ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
};

class AbstractClassWithMethods : public System::Object
{
    typedef AbstractClassWithMethods ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    virtual void SomeAbstractMethod() = 0;
    virtual void SomeVirtualMethod();
    void SomeMethod();
    
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "AbstractClasses.h"

namespace TypesPorting {

RTTI_INFO_IMPL_HASH(1064729720u, ::TypesPorting::AbstractClassWithoutMethods, ThisTypeBaseTypesInfo);

RTTI_INFO_IMPL_HASH(2860880958u, ::TypesPorting::AbstractClassWithMethods, ThisTypeBaseTypesInfo);

void AbstractClassWithMethods::SomeVirtualMethod()
{
}

void AbstractClassWithMethods::SomeMethod()
{
}

} // namespace TypesPorting

{{< /highlight >}}
