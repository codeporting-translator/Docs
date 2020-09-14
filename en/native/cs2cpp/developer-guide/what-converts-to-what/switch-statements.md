---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Switch Statements"
linktitle: "Switch Statements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "37"
lastmod: "2019-05-28"
weight: "37"
---

This example demonstrates how switch statement is ported to C++. Switch statement with stings is translated to C++ if statement.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_SwitchStatements.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_SwitchStatements_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_SwitchStatements.cpp">}}
