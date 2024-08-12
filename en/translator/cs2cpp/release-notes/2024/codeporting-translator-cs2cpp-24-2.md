---
date: "2024-02-16"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 24.2"
linktitle: "CodePorting.Translator Cs2Cpp 24.2"
menu:
  docs:
    parent: "2024"
    weight: "7"
lastmod: "2024-02-16"
weight: "7"
---

## Major Features ##

None.

## Minor fixes ##

1. `File::SetLastWriteTime` now throws an `ArgumentOutOfRangeException` if passed time is invalid.
1. `Path::GetTempFileName` creates unique file now, not only generates file name.
1. `System::String` indexer now throws `IndexOutOfRangeException` when index is out of range.
1. `CppSkipEntityAttribute` now applies to structures.
1. Added basic `System::Activator` support.
1. Translation for `Color.IsNamedColor` property is implemented.

## Public API and Backward Incompatible Changes ##

None.
