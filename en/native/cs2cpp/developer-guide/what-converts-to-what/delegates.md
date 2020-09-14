---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Delegates"
linktitle: "Delegates"
menu:
  docs:
    parent: "What Converts to What"
    weight: "6"
lastmod: "2019-05-28"
weight: "6"
---

This example demonstrates how C# delegates are ported to C++. They are declared using special type System::MulticastDelegate<T> from asposecpplib.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist codeporting-com-gists 3bd44df83922c0ca4e4e8948cee8099b "Codeportingcpp_Csharp_Delegate.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_Delegate_Header.cpp">}}
