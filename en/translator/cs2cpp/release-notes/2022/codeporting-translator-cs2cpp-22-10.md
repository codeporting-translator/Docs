---
date: "2022-10-10"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 22.10"
linktitle: "CodePorting.Translator Cs2Cpp 22.10"
menu:
  docs:
    parent: "2022"
    weight: "1"
lastmod: "2022-10-10"
weight: "1"
---

## Major Features ##

1. The translator now generates new `ExplicitCast` and `AsCast` methods instead of the obsolete `DynamicCast`, `DynamicCast_noexcept`, `StaticCast` and `StaticCast_noexcept`.
1. A new `generate_struct_default_methods` option has been added. This option forces the translator to generate default methods for all structures in the project.

## Minor Fixes ##

1. All structures in the translated code are now considered `IsBoxable`.
1. The `FILETIME` structure now supports comparison using the `Equals` method.
1. The automatic generation of the default methods for structs is fixed.
1. The search inside ZipFile is optimized.
1. A warning is added for unexpected whitespace in translator command arguments.

## Full List of Issues Covering all Changes in this Release ##

| Key          | Summary | Category |
|--------------| --- |----------|
|CSPORTCPP-2133|Improve casting translation|Task|
|CSPORTCPP-5573|Automatic boxing for all structures|Task|
|CSPORTCPP-5627|Automatic default members for all structures|Task|
|SLIDESCPP-3548|Application hangs when loading a presentation|Bug|
|CSPORTCPP-5635|Translator fails if has end-of-line in command arguments.|Bug|

## Public API and Backward Incompatible Changes ##

1. The `DynamicCast`, `DynamicCast_noexcept`, `StaticCast`, `StaticCast_noexcept` functions are now marked as deprecated and will be removed in the upcoming releases. The `ExplicitCast` and `AsCast` functions may be used instead of them. The translator now generates a code that uses the new casts.
1. Attributes `[CodePorting.Translator.Cs2Cpp.CppForceDynamicCastFromTypeParam]`, `[CodePorting.Translator.Cs2Cpp.CppForceDynamicCastToTypeParam]` and `[CodePorting.Translator.Cs2Cpp.CppUnknownTypeParam]` are marked as obsolete and should no longer be used.
