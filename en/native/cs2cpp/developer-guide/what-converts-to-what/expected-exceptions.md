---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Expected Exceptions"
linktitle: "Expected Exceptions"
menu:
  docs:
    parent: "What Converts to What"
    weight: "12"
lastmod: "2019-05-28"
weight: "12"
---

This example demonstrates how NUnit test with expected exception attribute applied is ported to C++. Googletest C++ library is used to translate NUnit tests to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_ExpectedException.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_ExpectedException_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_ExpectedException.cpp">}}
