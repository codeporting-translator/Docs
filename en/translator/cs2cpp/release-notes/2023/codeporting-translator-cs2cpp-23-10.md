---
date: "2023-10-11"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.10"
linktitle: "CodePorting.Translator Cs2Cpp 23.10"
menu:
  docs:
    parent: "2023"
    weight: "2"
lastmod: "2023-10-11"
weight: "3"
---

## Major Features ##

None.

## Minor fixes ##

1. Overriding a method implementation with explicit C++ code no longer requires adding the `CppSkipDefinition` attribute.
1. `System::HashSet` now has a constructor with a specified capacity.
1. `System::Net::Http::HttpClient` default constructor is implemented.
1. `Object.ReferenceEquals` for strings is fixed and works properly for almost all cases.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| TASKSCPP-1885 | Fix implicit "implementation" - remove CppSkipDefinition dependency and required implementation code. | Task |
| TASKSCPP-1889 | Add possibility to create HashSet with a specified capacity | Task |
| PDFCPP-2524 | MeasureCash.Init issue | Task |

## Public API and Backward Incompatible Changes ##

None.
