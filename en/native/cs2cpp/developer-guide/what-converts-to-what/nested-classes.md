---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Nested Classes"
linktitle: "Nested Classes"
menu:
  docs:
    parent: "What Converts to What"
    weight: "25"
lastmod: "2019-05-28"
weight: "25"
---

This example demonstrates how C# nested classes are ported to C++. Public, protected and private classes preserve their accessibility level. Internal classes become protected.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_NestClasses.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_NestedClasses_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_NestedClasses.cpp">}}
