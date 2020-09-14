---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Generic Methods"
linktitle: "Generic Methods"
menu:
  docs:
    parent: "What Converts to What"
    weight: "19"
lastmod: "2019-05-28"
weight: "19"
---

This example demonstrates how generic class methods are ported to C++. In C++ they become template methods.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_GenericMethods.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_GenericMethods_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_GenericMethods.cpp">}}
