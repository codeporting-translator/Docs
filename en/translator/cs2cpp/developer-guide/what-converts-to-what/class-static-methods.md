---
date: "2022-11-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassStaticMethods"
linktitle: "ClassStaticMethods"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-11-10"
weight: "1"
---

This example demonstrates how class static methods are translated to C++. Public, protected and private static methods preserve their accessibility level. Internal static methods become protected.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassStaticMethods
    {
        public static void PublicMethod()
        {
        }

        internal static void InternalMethod()
        {
        }

        protected static void ProtectedMethod()
        {
        }

        private static void PrivateMethod()
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

namespace MembersPorting {

class ClassStaticMethods : public System::Object
{
    typedef ClassStaticMethods ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    static void PublicMethod();
    
protected:

    static void InternalMethod();
    static void ProtectedMethod();
    
private:

    static void PrivateMethod();
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassStaticMethods.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(3437933940u, ::MembersPorting::ClassStaticMethods, ThisTypeBaseTypesInfo);

void ClassStaticMethods::PublicMethod()
{
}

void ClassStaticMethods::InternalMethod()
{
}

void ClassStaticMethods::ProtectedMethod()
{
}

void ClassStaticMethods::PrivateMethod()
{
}

} // namespace MembersPorting

{{< /highlight >}}
