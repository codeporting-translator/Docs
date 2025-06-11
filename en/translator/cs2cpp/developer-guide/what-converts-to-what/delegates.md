---
date: "2025-06-10"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "Delegates"
linktitle: "Delegates"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2025-06-10"
weight: "1"
---

This example demonstrates how delegates are translated to C++. They are declared using special type System::MulticastDelegate<T> from asposecpplib.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
namespace TypesPorting
{
    public delegate void Delegate1();
    public delegate int Delegate2();
    public delegate void Delegate3(string arg1, int arg2);
    public delegate int Delegate4(string arg1, int arg2);
    public delegate int Delegate5(out string arg1, ref int arg2);
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/string.h>
#include <system/multicast_delegate.h>
#include <cstdint>

namespace TypesPorting {

using Delegate1 = System::MulticastDelegate<void()>;
using Delegate2 = System::MulticastDelegate<int32_t()>;
using Delegate3 = System::MulticastDelegate<void(System::String, int32_t)>;
using Delegate4 = System::MulticastDelegate<int32_t(System::String, int32_t)>;
using Delegate5 = System::MulticastDelegate<int32_t(System::String&, int32_t&)>;
} // namespace TypesPorting



{{< /highlight >}}
