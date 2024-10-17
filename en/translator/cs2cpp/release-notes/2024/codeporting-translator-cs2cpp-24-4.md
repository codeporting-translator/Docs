---
date: "2024-04-18"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 24.4"
linktitle: "CodePorting.Translator Cs2Cpp 24.4"
menu:
  docs:
    parent: "2024"
    weight: "7"
lastmod: "2024-04-18"
weight: "7"
---

## Major Features ##

1. The translator and library now support compilation for C++17 and C++20.
1. `System::Collections::Generic::SortedSet` container class implementation is added.
1. The `System::Threading::Mutex` implementation has been completely redesigned and brought closer to the behavior in .NET.

## Minor fixes ##

1. Missing overload for `String::ConvertToUtf32` method is added.
1. Closing of base stream in `StreamReader` class is fixed.
1. Minimal CMake version for translated C# projects is raised to 3.18.
1. Methods `Char::ToUpper` and `Char::ToLower` now take `CultureInfo` parameter into account.

## Public API and Backward Incompatible Changes ##

None.
