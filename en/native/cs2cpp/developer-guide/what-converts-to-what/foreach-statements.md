---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "ForEach Statements"
linktitle: "ForEach Statements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "15"
lastmod: "2019-05-28"
weight: "15"
---

This example demonstrates how foreach statement is ported to C++. It is translated to C++ for statement.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_ForEachStatements.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_ForEachStatements_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_ForEachStatements.cpp">}}
