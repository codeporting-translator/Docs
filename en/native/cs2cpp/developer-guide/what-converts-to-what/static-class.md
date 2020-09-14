---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Static Class"
linktitle: "Static Class"
menu:
  docs:
    parent: "What Converts to What"
    weight: "33"
lastmod: "2019-05-28"
weight: "33"
---

This example demonstrates how static C# classes are ported to C++. They are not inherited from System::Object unlike others and have no RTTI declared.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_StaticClass.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_StaticClass_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_StaticClass.cpp">}}
