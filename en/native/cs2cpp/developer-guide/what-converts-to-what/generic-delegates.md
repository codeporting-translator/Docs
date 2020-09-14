---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Generic Delegates"
linktitle: "Generic Delegates"
menu:
  docs:
    parent: "What Converts to What"
    weight: "17"
lastmod: "2019-05-28"
weight: "17"
---

This example demonstrates how C# generic delegates are ported to C++. They are declared using special type System::MulticastDelegate<T> from asposecpplib.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_GenericDelegates.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_GenericDelegates_Header.cpp">}}
