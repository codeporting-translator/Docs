---
date: "2024-11-19"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 24.11"
linktitle: "CodePorting.Translator Cs2Cpp 24.11"
menu:
  docs:
    parent: "2024"
    weight: "1"
lastmod: "2024-10-19"
weight: "1"
---

## Major Features ##

None.

## Minor fixes ##

1. Added required #include generation while using `ASPOSE_ASSERT_ISINSTANCEOF(...)` macro.
1. Implementation of method `ConcurrentDictionary::TryAdd` is added.
1. Added a base class for implementations of the `System::Collections::Generic::IComparer` generic interface.
1. Improved LINQ support: `Aggregate`, `ElementAtOrDefault`, `Reverse`, `Take`, `Max`, `Min`, `ThenBy` methods are implemented now.
1. Interfaces `IEqualityComparer`, `IOrderedEnumerable` are supported now.
1. Metainformation support for enums is improved.

## Public API and Backward Incompatible Changes ##

None.
