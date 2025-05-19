---
date: "2025-05-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ClassIndexers"
linktitle: "ClassIndexers"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2025-05-10"
weight: "1"
---

This example demonstrates how class indexers are translated to C++. Public, protected and private indexers preserve their accessibility level. Internal indexers become protected. Each indexer is translated into two methods: idx_get and idx_set.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace MembersPorting
{
    public class ClassIndexers
    {
        public int this[int index]
        {
            get { return 0; }
            set { /* do nothing */ }
        }

        internal string this[string index]
        {
            get { return "iddqd"; }
            set { /* do nothing */ }
        }

        protected bool this[bool index]
        {
            get { return true; }
            set { /* do nothing */ }
        }

        private object this[object index]
        {
            get { return new object(); }
            set { /* do nothing */ }
        }
    }
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <cstdint>

namespace System
{
class String;
} // namespace System

namespace MembersPorting {

class ClassIndexers : public System::Object
{
    typedef ClassIndexers ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    int32_t idx_get(int32_t index);
    void idx_set(int32_t index, int32_t value);
    
protected:

    System::String idx_get(System::String index);
    void idx_set(System::String index, System::String value);
    bool idx_get(bool index);
    void idx_set(bool index, bool value);
    
private:

    System::SharedPtr<System::Object> idx_get(System::SharedPtr<System::Object> index);
    void idx_set(System::SharedPtr<System::Object> index, System::SharedPtr<System::Object> value);
    
};

} // namespace MembersPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ClassIndexers.h"

#include <system/string.h>

namespace MembersPorting {

RTTI_INFO_IMPL_HASH(77152164u, ::MembersPorting::ClassIndexers, ThisTypeBaseTypesInfo);

int32_t ClassIndexers::idx_get(int32_t index)
{
    return 0;
}

void ClassIndexers::idx_set(int32_t index, int32_t value)
{
    /* do nothing */
}

System::String ClassIndexers::idx_get(System::String index)
{
    return u"iddqd";
}

void ClassIndexers::idx_set(System::String index, System::String value)
{
    /* do nothing */
}

bool ClassIndexers::idx_get(bool index)
{
    return true;
}

void ClassIndexers::idx_set(bool index, bool value)
{
    /* do nothing */
}

System::SharedPtr<System::Object> ClassIndexers::idx_get(System::SharedPtr<System::Object> index)
{
    return System::MakeObject<System::Object>();
}

void ClassIndexers::idx_set(System::SharedPtr<System::Object> index, System::SharedPtr<System::Object> value)
{
    /* do nothing */
}

} // namespace MembersPorting

{{< /highlight >}}
