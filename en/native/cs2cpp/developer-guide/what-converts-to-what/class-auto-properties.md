---
date: "2021-08-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassAutoProperties"
linktitle: "ClassAutoProperties"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-08-09"
weight: "1"
---

This example demonstrates how class auto properties are ported to C++. Public, protected and private properties preserve their accessibility level. Internal properties become protected. Getters are ported to  methods with prefix get_ and setters with prefix set_.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassAutoProperties
    {
        public int PublicProperty { get; set; }

        internal string InternalProperty { get; set; }

        protected bool ProtectedProperty { get; set; }

        private double PrivateProperty { get; set; }
    }
}

{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/string.h>
#include <cstdint>

namespace MembersPorting {

class ClassAutoProperties : public System::Object
{
    typedef ClassAutoProperties ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    int32_t get_PublicProperty();
    void set_PublicProperty(int32_t value);
    
    ClassAutoProperties();
    
protected:

    System::String get_InternalProperty();
    void set_InternalProperty(System::String value);
    bool get_ProtectedProperty();
    void set_ProtectedProperty(bool value);
    
private:

    int32_t pr_PublicProperty;
    System::String pr_InternalProperty;
    bool pr_ProtectedProperty;
    double pr_PrivateProperty;
    
    double get_PrivateProperty();
    void set_PrivateProperty(double value);
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassAutoProperties.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(162763058u, ::MembersPorting::ClassAutoProperties, ThisTypeBaseTypesInfo);

int32_t ClassAutoProperties::get_PublicProperty()
{
    return pr_PublicProperty;
}

void ClassAutoProperties::set_PublicProperty(int32_t value)
{
    pr_PublicProperty = value;
}

System::String ClassAutoProperties::get_InternalProperty()
{
    return pr_InternalProperty;
}

void ClassAutoProperties::set_InternalProperty(System::String value)
{
    pr_InternalProperty = value;
}

bool ClassAutoProperties::get_ProtectedProperty()
{
    return pr_ProtectedProperty;
}

void ClassAutoProperties::set_ProtectedProperty(bool value)
{
    pr_ProtectedProperty = value;
}

double ClassAutoProperties::get_PrivateProperty()
{
    return pr_PrivateProperty;
}

void ClassAutoProperties::set_PrivateProperty(double value)
{
    pr_PrivateProperty = value;
}

ClassAutoProperties::ClassAutoProperties() : pr_PublicProperty(0), pr_ProtectedProperty(false), pr_PrivateProperty(0)
{
}

} // namespace MembersPorting

{{< /highlight >}}
