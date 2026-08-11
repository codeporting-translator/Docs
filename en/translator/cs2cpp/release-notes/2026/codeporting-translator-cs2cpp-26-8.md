---
date: "2026-08-11"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 26.8"
linktitle: "CodePorting.Translator Cs2Cpp 26.8"
menu:
  docs:
    parent: "2026"
    weight: "1"
lastmod: "2026-08-11"
weight: "1"
---

## Major Features ##

1. Implemented the `System::Memory<T>` class and all related code infrastructure.
1. Added support for UTF-8 string literals.
1. Added a new `CppFragment` attribute for manual control over syntax node translation.
1. Implemented translation for **ref** fields and fixed **ref** local variables. They now translate to raw C++ pointers, not C++ references, and can be reassigned (as in C#).

## Minor fixes ##

1. Fixed the `force_const_ref_parameters` option with generic delegates.
1. Refactored config loading and improved diagnostics for obsolete options.
1. Improved diagnostics for obsolete and unused attributes.
1. Added support for `ExifOrientation` in JPEG images.
1. Added command-string control for log severity and log time format.
