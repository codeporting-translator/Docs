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
    weight: "1"
lastmod: "2023-06-15"
weight: "1"
---

## Major Features ##

None.

## Minor fixes ##

1. Fixed initialization of `Dictionary` with delegate in key or value.
1. `FileAttributes::Normal` is set when `ZipEntry` is created from the stream.
1. Option `reference_configurations' is now supported.
1. Synchronized `System::Random` class with .NET equivalent and fixed its inheritance bug.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3740 | Fixed initialization of Dictionary with delegate in key or value | Task |
| PDFCPP-2346-ZipEntry_default_attributes | set FileAttributes::Normal when ZipEntry created from stream | Task |
| PDFCPP-2284_ReferencesConfigurations2 | option 'references_configurations' | Task |
| SLIDESCPP-3730 | Fixed bug on System::Random inheritance | Task |
| SLIDESCPP-3723 | Fixed mistakes in Doxygen comments | Bug |
| SLIDESCPP-3730 | Synchronized System::Random class with .NET equivalent | Task |

## Public API and Backward Incompatible Changes ##

None.
