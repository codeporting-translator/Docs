---
date: "2023-09-15"
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
lastmod: "2023-09-15"
weight: "1"
---

## Major Features ##

None.

## Minor fixes ##

1. Fixed `Graphics::set_Clip` method when region is empty.
1. Absolute paths to `fontconfig` and `freetype` libraries in macOS arm64 version of asposecpplib are replaced with @rpath's.
1. Fixed an undefined behavior when cloning regions.
1. Generic call `Assert.IsInstanceOf<typename>` is translating correctly now.
1. Fixed some fonts families logic for Emscripten builds.
1. Fixed simultaneous usage of `generate_includes_subdirectory` and `force_public_headers` options.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| PDFCPP-2464 | RegressionTests_v23_05.PDFNET_51072 | Task |
| PDFCPP-2467 | Fix the problem of "Full paths to fontconfig and freetype libraries" in macOS arm64 version of asposecpplib | Task |
| SLIDESCPP-3785 | Incorrect metafile rendering (ARM64) | Task |
| PDFCPP-2493 | porting Assert.IsInstanceOf\<TypeName\> | Task |
| PDFCPP-2488 | Investigate PDF to PPTX conversion and implement in Aspose.PDF for JavaScript via C++ | Task |
| TASKSCPP-1863 | "Filter.h" (Aspose.Tasks.CPP) conflicts with the "filter.h" ( Windows SDK include) | Task |

## Public API and Backward Incompatible Changes ##

None.
