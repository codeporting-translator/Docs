---
date: "2023-06-15"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.6"
linktitle: "CodePorting.Translator Cs2Cpp 23.6"
menu:
  docs:
    parent: "2023"
    weight: "2"
lastmod: "2023-06-15"
weight: "2"
---

## Major Features ##

None.

## Minor fixes ##

1. Enum operations with type arguments are supported now.
1. Fixed initialization of `Dictionary` with delegate in key or value.
1. `FileAttributes::Normal` is set when `ZipEntry` is created from the stream.
1. Option `reference_configurations` is now supported.
1. Synchronized `System::Random` class with .NET equivalent.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| FONTCPP-123 | issue of enum methods with type arguments | Task |
| SLIDESCPP-3740 | Porter: Incorrect initialization of dictionaries with delegates | Task |
| PDFCPP-2346 | Fix PptxExportTests.PDFNET_50458 | Task |
| PDFCPP-2284 | CsToCppPorter - parse referenced dependencies in right configurations | Task |
| SLIDESCPP-3723 | Fix the generation warnings of the updated API Reference | Task |
| SLIDESCPP-3730 | Synchronize System::Random class with .NET equivalent | Task |

## Public API and Backward Incompatible Changes ##

None.
