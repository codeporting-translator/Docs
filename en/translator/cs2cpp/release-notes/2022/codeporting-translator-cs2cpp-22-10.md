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

1. Now the translator generates new `ExplicitCast` and `AsCast` methods instead of obolete `DynamicCast`, `DynamicCast_noexcept`, `StaticCast` and `StaticCast_noexcept`.
1. New `generate_struct_default_methods` option is added. This option forces translator to generate default methods for all structs in the project.

## Minor Fixes ##

1. All structures in trsnalted code are considered `IsBoxable` now.
1. `FILETIME` structure now supports comparation using `Equals` method.
1. Automatic struct default methods genearation is fixed now.
1. The Zip search code is optimized.
1. Added warning for unexpected whitespaces in translator command arguments.

## Full List of Issues Covering all Changes in this Release ##

| Key          | Summary | Category |
|--------------| --- |----------|
|CSPORTCPP-2133|Improve casting translation|Task|
|CSPORTCPP-5573|Automatic boxing for all structures|Task|
|CSPORTCPP-5627|Automatic default members for all structures|Task|
|SLIDESCPP-3548|Optimization of searching in ZipFile|Task|
|CSPORTCPP-5635|Translator fails if has end-of-line in command arguments.|Task|

## Public API and Backward Incompatible Changes ##

1. The `DynamicCast`, `DynamicCast_noexcept`, `StaticCast`, `StaticCast_noexcept` functions now marked as deprecated and will be removed in the upcoming releases. The `ExplicitCast` and `AsCast` functions may be used instead of them. The translator now generate a code that uses the new casts.
1. Attributes `[CodePorting.Translator.Cs2Cpp.CppForceDynamicCastFromTypeParam]`, `[CodePorting.Translator.Cs2Cpp.CppForceDynamicCastToTypeParam]` and '[CodePorting.Translator.Cs2Cpp.CppUnknownTypeParam]' are marked as obsolete and should no longer be used.
