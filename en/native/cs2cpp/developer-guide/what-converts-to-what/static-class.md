---
date: "2021-09-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "StaticClass"
linktitle: "StaticClass"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-09-10"
weight: "1"
---

This example demonstrates how static classes are ported to C++. They are not inherited from System::Object unlike others and have no RTTI declared.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public static class StaticClass
    {
        public static void StaticMethod()
        {
        }
    }
}
{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

namespace TypesPorting {

class StaticClass
{
    typedef StaticClass ThisType;
    
public:

    static void StaticMethod();
    
public:
    StaticClass() = delete;
};

} // namespace TypesPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "StaticClass.h"

namespace TypesPorting {

void StaticClass::StaticMethod()
{
}

} // namespace TypesPorting

{{< /highlight >}}
