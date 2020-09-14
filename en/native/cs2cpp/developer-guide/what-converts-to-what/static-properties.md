---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Static Properties"
linktitle: "Static Properties"
menu:
  docs:
    parent: "What Converts to What"
    weight: "36"
lastmod: "2019-05-28"
weight: "36"
---

This example demonstrates how class static properties are ported. Static properties may have getter, setter, or both. Public, protected and private static properties preserve their accessibility level. Internal static properties become protected. Getters are ported to static methods with prefix get_ and setters with prefix set_.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist csportertotal 2835382f1599d4367c1fb19f46dd15ae "csPortercpp_Csharp_StaticProperties.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_StaticProperties_Header.cpp">}}

### C++ Source Code ###

{{< gist csportertotal 828e6770a3d27de2e78022affa71bfbf "csPortercpp_Cpp_StaticProperties.cpp">}}
