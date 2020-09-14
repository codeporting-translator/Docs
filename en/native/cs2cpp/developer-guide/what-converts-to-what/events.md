---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Events"
linktitle: "Events"
menu:
  docs:
    parent: "What Converts to What"
    weight: "10"
lastmod: "2019-05-28"
weight: "10"
---

This example demonstrates how C# class events are ported to C++. Public, protected and private class events preserve their accessibility level. Internal class events become protected. Events are ported to fields of type System::Event<T> which is a part of asposecpplib.

Additional command-line options passed to CsToCppPorter: none.

## Source Code ##

{{< gist codeporting-com-gists 3bd44df83922c0ca4e4e8948cee8099b "Codeportingcpp_Csharp_Events.cs">}}

## Ported Code ##

### C++ Header ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_Events_Header.cpp">}}

### C++ Source Code ###

{{< gist codeporting-com-gists 21b1d9f813c2545e2d860fbc26365563 "Codeportingcpp_Cpp_Events.cpp">}}
