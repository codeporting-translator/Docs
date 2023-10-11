---
date: "2023-07-10"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.7"
linktitle: "CodePorting.Translator Cs2Cpp 23.7"
menu:
  docs:
    parent: "2023"
    weight: "4"
lastmod: "2023-07-10"
weight: "4"
---

## Major Features ##

1. MacOS Arm64 architecture is supported now. The asposecpplib build for this architecture has been added to the release.

## Minor fixes ##

1. Initialization of dictionaries with overloaded methods is fixed.
1. `goto case` statement for switch with string argument implemented properly.
1. Fixed named mutexes implementation for MacOS platform.
1. Fixed incorrect drawing of math symbols under MacOS.
1. Nested `@cond` / `@endcond` tags are removed.
1. Incorrect initialization of lists with delegate is fixed.
1. Visual Studio version name can be selected conditionally over the environment variable `Cs2CppTranslatorForceVsInstanceVersion` now.
1. Added missing `Regex::Replace` method implementation.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3748 | Porter: Incorrect initialization of dictionaries with delegates #2 | Task |
| CSPORTCPP-5848 | Support Mac M1 ARM architecture build. | Task |
| PDFCPP-2376 | Issue with porting Switch statement with goto operators | Task |
| CSPORTCPP-5907 | MacOS NamedMutex tests fails if other user already created boost_interprocess temporary folder. | Bug |
| SLIDESCPP-3696 | Incorrect drawing of math symbols (macOS) | Task |
| SLIDESCPP-3751 | Incorrect placement of methods in API References | Task |
| SLIDESCPP-3742 | Porter: Incorrect initialization of lists with delegates | Task |
| SLIDESCPP-3758 | Porter: Fix problem with Microsoft.NET.StringTools exception | Task |

## Public API and Backward Incompatible Changes ##

1. The MacOS X86_64 build now has the `appleclang_x86_64` suffix. Build for Arm64, respectively, has the `appleclang_arm64` suffix. The short suffix `appleclang` will have a universal build, which is planned for the next release.
