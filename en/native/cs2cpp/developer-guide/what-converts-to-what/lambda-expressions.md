---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Lambda Expressions"
linktitle: "Lambda Expressions"
menu:
  docs:
    parent: "What Converts to What"
    weight: "23"
lastmod: "2019-05-28"
weight: "23"
---

This example demonstrates how C# lambda expressions are ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_Lambda.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_Lambda_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_Lambda.cpp">}}
