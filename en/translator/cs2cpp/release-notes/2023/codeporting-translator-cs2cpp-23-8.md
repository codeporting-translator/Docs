---
date: "2023-08-10"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.8"
linktitle: "CodePorting.Translator Cs2Cpp 23.8"
menu:
  docs:
    parent: "2023"
    weight: "1"
lastmod: "2023-08-10"
weight: "1"
---

## Major Features ##

None.

## Minor fixes ##

1. Recovery mode is added for `ZipFile` and fixed bug with multiple `FileStream` pointers.
1. Fixed parametrized `Array.Sort` method translation.
1. Fixed wrap mode for gradient brushes.
1. Fixed `System::IO::File::SetLastWriteTimeUtc` call on directory.
1. `WebHeaderCollection::IsRestricted` method is implemented.
1. Namespace `icu` replaced with `codeporting_icu` to avoid conflict when asposecpplib used together with another ICU version.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3741 | Fix RegressionTests_v23_6.SLIDESNET_43807 test | Task |
| SLIDESCPP-3004 | Porter: Fix parametrized Array.Sort method translation | Task |
| PAGECPP-121 | issue LinearGradientBrush.WrapMode in asposecpplib | Task |
| PDFCPP-2409 | PptxExportTests.PDFNET_50458 - Error writing file's attributes: docProps | Task |
| PDFCPP-2413 | Implement WebHeaderCollection::IsRestricted method | Task |
| TASKSCPP-1863 | "Filter.h" (Aspose.Tasks.CPP) conflicts with the "filter.h" ( Windows SDK include) | Task |

## Public API and Backward Incompatible Changes ##

None.
