---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Try Catch Statements"
linktitle: "Try Catch Statements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "41"
lastmod: "2019-05-28"
weight: "41"
---

This example demonstrates how C# try-catch statement is ported to C++.

Additional command-line options passed to CsToCppPorter: -o finally_statement_as_lambda#true.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_TryCatchStatements.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_TryCatchStatements_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_TryCatchStatements.cpp">}}
