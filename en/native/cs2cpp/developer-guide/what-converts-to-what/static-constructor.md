---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Static Constructor"
linktitle: "Static Constructor"
menu:
  docs:
    parent: "What Converts to What"
    weight: "34"
lastmod: "2019-05-28"
weight: "34"
---

This example demonstrates how C# static class constructor is ported to C++. A static field with special name s_constructor~_~_ of special type **StaticConstructor** is added. The code is executed on program startup and this behavior is different from C# static constructors which are called right before first class usage.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_StaticConstructors.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_StaticConstructors_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_StaticConstructors.cpp">}}
