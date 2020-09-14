---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Simple Struct"
linktitle: "Simple Struct"
menu:
  docs:
    parent: "What Converts to What"
    weight: "30"
lastmod: "2019-05-28"
weight: "30"
---

This example demonstrates how simple C# struct is ported to C++. A result C++ class inherits System::Object class and contains definitions for RTTI.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_SimpleStructs.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_SimpleStructs_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_SimpleStructs.cpp">}}
