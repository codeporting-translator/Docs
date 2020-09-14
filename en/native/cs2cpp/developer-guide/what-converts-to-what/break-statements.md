---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Break Statements"
linktitle: "Break Statements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "3"
lastmod: "2019-05-28"
weight: "3"
---

This example demonstrates how C# break statement inside loop is ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist codeporting-com-gists 3bd44df83922c0ca4e4e8948cee8099b "Codeportingcpp_Csharp_BreakStatements.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_BreakStatements_Header.cpp">}}

### C++ Source Code ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_BreakStatements.cpp">}}
