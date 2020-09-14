---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Methods"
linktitle: "Methods"
menu:
  docs:
    parent: "What Converts to What"
    weight: "24"
lastmod: "2019-05-28"
weight: "24"
---

This example demonstrates how class methods are ported to C++. Public, protected and private methods preserve their accessibility level. Internal methods become protected.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_Methods.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_Methods_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_Methods.cpp">}}
