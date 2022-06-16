---
date: "2022-06-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassProperties"
linktitle: "ClassProperties"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-06-09"
weight: "1"
---

This example demonstrates how class properties which have explicitly defined setters and getters are ported. Properties may have getter, setter, or both. Public, protected and private properties preserve their accessibility level. Internal properties become protected. Getters are ported to methods with prefix get_ and setters with prefix set_.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassProperties
    {
        public int PublicProperty
        {
            get { return mPublicPropertyField; }
            set { mPublicPropertyField = value; }
        }

        public int PublicPropertyWithoutSetter
        {
            get { return mPublicPropertyField; }
        }

        public int PublicPropertyWithoutGetter
        {
            set { mPublicPropertyWithoutGetterField = value; }
        }

        internal string InternalProperty
        {
            get { return mInternalPropertyField; }
            set { mInternalPropertyField = value; }
        }

        protected bool ProtectedProperty
        {
            get { return mProtectedPropertyField; }
            set { mProtectedPropertyField = value; }
        }

        private double PrivateProperty
        {
            get { return mPrivatePropertyField; }
            set { mPrivatePropertyField = value; }
        }

        private int mPublicPropertyField;
        private int mPublicPropertyWithoutSetterField;
        private int mPublicPropertyWithoutGetterField;
        private string mInternalPropertyField;
        private bool mProtectedPropertyField;
        private double mPrivatePropertyField;
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

class ClassProperties : public System::Object
{
    typedef ClassProperties ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    int32_t get_PublicProperty();
    void set_PublicProperty(int32_t value);
    int32_t get_PublicPropertyWithoutSetter();
    void set_PublicPropertyWithoutGetter(int32_t value);
    
    ClassProperties();
    
protected:

    System::String get_InternalProperty();
    void set_InternalProperty(System::String value);
    bool get_ProtectedProperty();
    void set_ProtectedProperty(bool value);
    
private:

    double get_PrivateProperty();
    void set_PrivateProperty(double value);
    
    int32_t mPublicPropertyField;
    int32_t mPublicPropertyWithoutSetterField;
    int32_t mPublicPropertyWithoutGetterField;
    System::String mInternalPropertyField;
    bool mProtectedPropertyField;
    double mPrivatePropertyField;
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassProperties.h"

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(541627203u, ::MembersPorting::ClassProperties, ThisTypeBaseTypesInfo);

int32_t ClassProperties::get_PublicProperty()
{
    return mPublicPropertyField;
}

void ClassProperties::set_PublicProperty(int32_t value)
{
    mPublicPropertyField = value;
}

int32_t ClassProperties::get_PublicPropertyWithoutSetter()
{
    return mPublicPropertyField;
}

void ClassProperties::set_PublicPropertyWithoutGetter(int32_t value)
{
    mPublicPropertyWithoutGetterField = value;
}

System::String ClassProperties::get_InternalProperty()
{
    return mInternalPropertyField;
}

void ClassProperties::set_InternalProperty(System::String value)
{
    mInternalPropertyField = value;
}

bool ClassProperties::get_ProtectedProperty()
{
    return mProtectedPropertyField;
}

void ClassProperties::set_ProtectedProperty(bool value)
{
    mProtectedPropertyField = value;
}

double ClassProperties::get_PrivateProperty()
{
    return mPrivatePropertyField;
}

void ClassProperties::set_PrivateProperty(double value)
{
    mPrivatePropertyField = value;
}

ClassProperties::ClassProperties() : mPublicPropertyField(0), mPublicPropertyWithoutSetterField(0)
    , mPublicPropertyWithoutGetterField(0), mProtectedPropertyField(false), mPrivatePropertyField(0)
{
}

} // namespace MembersPorting

{{< /highlight >}}
