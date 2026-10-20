---
date: "2026-10-17"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 26.10"
linktitle: "CodePorting.Translator Cs2Cpp 26.10"
menu:
  docs:
    parent: "2026"
    weight: "1"
lastmod: "2026-10-17"
weight: "1"
---

## Major Features ##

None.

## Minor fixes ##

1. Improved null string behavior to align with C# semantics.
1. Fixed `System::Span` assignment with `null`.
1. Added a **Tutorial** section to the online documentation.
1. Fixed the performance regression in gold tests.
1. Fixed polymorphic cloning configuration.
1. Fixed `Region::Clone()` throwing when a region contains an empty `GraphicsPath`.
1. Included cast types for **enums**.
1. Added `[CppConstRefParam]` attribute support for properties.
1. Fixed null string handling in font family names.
