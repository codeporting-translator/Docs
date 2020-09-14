---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Indexers"
linktitle: "Indexers"
menu:
  docs:
    parent: "What Converts to What"
    weight: "22"
lastmod: "2019-05-28"
weight: "22"
---

This example demonstrates how C# class indexers are ported to C++. Public, protected and private indexers preserve their accessibility level. Internal indexers become protected. Each indexer is ported into two methods: idx_get and idx_set.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_Indexers.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_Indexers_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_Indexers.cpp">}}
