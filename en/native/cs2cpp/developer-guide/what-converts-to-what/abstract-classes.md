---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Abstract Classes"
linktitle: "Abstract Classes"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2019-05-28"
weight: "1"
---

This example demonstrates how C# abstract classes are ported to C++. They are declared using macro ABSTRACT which expands into _declspec(novtable) for Microsoft VC++.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist codeporting-com-gists 3bd44df83922c0ca4e4e8948cee8099b "Codeportingcpp_Csharp_AbstractClass.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_AbstractClass_Header.cpp">}}


### C++ Source Code ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_AbstractClass.cpp">}}
