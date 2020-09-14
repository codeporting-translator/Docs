---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Auto Properties"
linktitle: "Auto Properties"
menu:
  docs:
    parent: "What Converts to What"
    weight: "2"
lastmod: "2019-05-28"
weight: "2"
---

This example demonstrates how class auto properties are ported to C++. Public, protected and private properties preserve their accessibility level. Internal properties become protected. Getters are ported to methods with prefix get_ and setters with prefix set_.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist codeporting-com-gists 3bd44df83922c0ca4e4e8948cee8099b "Codeportingcpp_Csharp_AutoProperties.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_AutoProperties_Header.cpp">}}

### C++ Source Code ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_AutoProperties.cpp">}}
