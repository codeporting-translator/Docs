---
date: "2022-03-14"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "NestedClasses"
linktitle: "NestedClasses"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-03-14"
weight: "1"
---

This example demonstrates how nested classes are ported to C++. Public, protected and private classes preserve their accessibility level. Internal classes become protected.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public class OuterClass
    {
        public class PublicNestedClass
        {
        }

        internal class InternalNestedClass
        {
        }

        protected class ProtectedNestedClass
        {
        }

        private class PrivateNestedClass
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

class OuterClass : public System::Object
{
    typedef OuterClass ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    class PublicNestedClass : public System::Object
    {
        typedef PublicNestedClass ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    };
    
    
protected:

    class InternalNestedClass : public System::Object
    {
        typedef InternalNestedClass ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    };
    
    class ProtectedNestedClass : public System::Object
    {
        typedef ProtectedNestedClass ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    };
    
    
private:

    class PrivateNestedClass : public System::Object
    {
        typedef PrivateNestedClass ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    };
    
    
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "NestedClasses.h"

namespace TypesPorting {

RTTI_INFO_IMPL_HASH(490849539u, ::TypesPorting::OuterClass::PrivateNestedClass, ThisTypeBaseTypesInfo);

RTTI_INFO_IMPL_HASH(3027911832u, ::TypesPorting::OuterClass::ProtectedNestedClass, ThisTypeBaseTypesInfo);

RTTI_INFO_IMPL_HASH(1087067711u, ::TypesPorting::OuterClass::InternalNestedClass, ThisTypeBaseTypesInfo);

RTTI_INFO_IMPL_HASH(918013299u, ::TypesPorting::OuterClass::PublicNestedClass, ThisTypeBaseTypesInfo);


RTTI_INFO_IMPL_HASH(615668933u, ::TypesPorting::OuterClass, ThisTypeBaseTypesInfo);

} // namespace TypesPorting

{{< /highlight >}}
