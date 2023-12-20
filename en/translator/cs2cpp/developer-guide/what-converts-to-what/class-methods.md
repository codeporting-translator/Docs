---
date: "2023-12-14"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassMethods"
linktitle: "ClassMethods"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2023-12-14"
weight: "1"
---

This example demonstrates how class methods are translated to C++. Public, protected and private methods preserve their accessibility level. Internal methods become protected.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassMethods
    {
        public void PublicMethod()
        {
        }

        internal void InternalMethod()
        {
        }

        protected void ProtectedMethod()
        {
        }

        private void PrivateMethod()
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

class ClassMethods : public System::Object
{
    typedef ClassMethods ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    void PublicMethod();
    
protected:

    void InternalMethod();
    void ProtectedMethod();
    
private:

    void PrivateMethod();
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassMethods.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(582355426u, ::MembersPorting::ClassMethods, ThisTypeBaseTypesInfo);

void ClassMethods::PublicMethod()
{
}

void ClassMethods::InternalMethod()
{
}

void ClassMethods::ProtectedMethod()
{
}

void ClassMethods::PrivateMethod()
{
}

} // namespace MembersPorting

{{< /highlight >}}
