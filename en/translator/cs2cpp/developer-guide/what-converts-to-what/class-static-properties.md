---
date: "2025-07-14"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassStaticProperties"
linktitle: "ClassStaticProperties"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2025-07-14"
weight: "1"
---

This example demonstrates how class static properties are translated. Static properties may have getter, setter, or both. Public, protected and private static properties preserve their accessibility level. Internal static properties become protected. Getters are translated to static methods with prefix get_ and setters with prefix set_.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassStaticProperties
    {
        public static int PublicProperty { get; set; }

        internal static string InternalProperty { get; set; }

        protected static int ProtectedProperty { get; set; }

        private static int PrivateProperty { get; set; }
    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/string.h>
#include <cstdint>

namespace MembersPorting {

class ClassStaticProperties : public System::Object
{
    typedef ClassStaticProperties ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    static int32_t get_PublicProperty();
    static void set_PublicProperty(int32_t value);
    
protected:

    static System::String get_InternalProperty();
    static void set_InternalProperty(System::String value);
    static int32_t get_ProtectedProperty();
    static void set_ProtectedProperty(int32_t value);
    
private:

    static int32_t pr_PublicProperty;
    static System::String pr_InternalProperty;
    static int32_t pr_ProtectedProperty;
    static int32_t pr_PrivateProperty;
    
    static int32_t get_PrivateProperty();
    static void set_PrivateProperty(int32_t value);
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassStaticProperties.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(4083627313u, ::MembersPorting::ClassStaticProperties, ThisTypeBaseTypesInfo);

int32_t ClassStaticProperties::pr_PublicProperty = 0;
System::String ClassStaticProperties::pr_InternalProperty;
int32_t ClassStaticProperties::pr_ProtectedProperty = 0;
int32_t ClassStaticProperties::pr_PrivateProperty = 0;

int32_t ClassStaticProperties::get_PublicProperty()
{
    return pr_PublicProperty;
}

void ClassStaticProperties::set_PublicProperty(int32_t value)
{
    pr_PublicProperty = value;
}

System::String ClassStaticProperties::get_InternalProperty()
{
    return pr_InternalProperty;
}

void ClassStaticProperties::set_InternalProperty(System::String value)
{
    pr_InternalProperty = value;
}

int32_t ClassStaticProperties::get_ProtectedProperty()
{
    return pr_ProtectedProperty;
}

void ClassStaticProperties::set_ProtectedProperty(int32_t value)
{
    pr_ProtectedProperty = value;
}

int32_t ClassStaticProperties::get_PrivateProperty()
{
    return pr_PrivateProperty;
}

void ClassStaticProperties::set_PrivateProperty(int32_t value)
{
    pr_PrivateProperty = value;
}

} // namespace MembersPorting

{{< /highlight >}}
