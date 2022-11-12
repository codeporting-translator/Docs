---
date: "2022-11-11"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 22.11"
linktitle: "CodePorting.Translator Cs2Cpp 22.11"
menu:
  docs:
    parent: "2022"
    weight: "1"
lastmod: "2022-11-11"
weight: "1"
---

## Major Features ##

1. Normalization, comparation and joining of strings are optimized.
1. SKIA library is updated. Now with disabled limits of image size.

## Minor Fixes ##

1. Removed `noexept` from iterators. Now iterators can throw exceptions.
1. Fixed `for_each_member` tool to show memory leaks for aliased pointers.
1. Added warning about undefined behaviour when `double` casts to `unsigned int`.
1. Fixed trasnlator crashes during multithreading translation on some projects.
1. Fixed `deferred_init` translator option.
1. Removed the dependency on the Botan library from the `System::Drawing` implementation.

## Full List of Issues Covering all Changes in this Release ##

| Key          | Summary | Category |
|--------------| --- |----------|
|PDFCPP-2043|Wrong exception handling in iterators|Task|
|PDFCPP-2044|Cast Double to UInt|Task|
|CSPORTCPP-1250|Speed up string operations|Enhancement|
|CSPORTCPP-5720|Translator crashes on some tests in multithreading mode.|Bug|
|SLIDESCPP-3546|Aspose.Slides for C++ returns null pointer instead of an image|Bug|
|SLIDESCPP-3586|Remove the dependency on the Botan library from the System::Drawing implementation|Task|
|PDFCPP-2040|Improve "for_each_member" tool to show memory leaks via aliased pointer's|Task|
|PDFCPP-2068|Fix "deferred_init" option in translator to generate correct C++ code.|Task|

## Public API and Backward Incompatible Changes ##

1. The `DynamicCast`, `DynamicCast_noexcept`, `StaticCast`, `StaticCast_noexcept` functions are deprecated and will be removed in the upcoming releases. The `ExplicitCast` and `AsCast` functions may be used instead of them.
