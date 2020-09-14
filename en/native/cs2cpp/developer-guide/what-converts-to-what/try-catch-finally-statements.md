---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Try Catch Finally Statements"
linktitle: "Try Catch Finally Statements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "40"
lastmod: "2019-05-28"
weight: "40"
---

This example demonstrates how try-catch-finally statement is ported to C++. They are translated to lambda expressions passed to System::DoTryFinally function.

Additional command-line options passed to CsToCppPorter: -o finally_statement_as_lambda#true.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_TryCatchFinallyStatements.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_TryCatchFinallyStatements_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_TryCatchFinallyStatements.cpp">}}
