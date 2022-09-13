---
date: "2022-09-13"
author:
  display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 22.9"
linktitle: "CodePorting.Translator Cs2Cpp 22.9"
menu:
  docs:
    parent: "2022"
    weight: "1"
lastmod: "2022-09-13"
weight: "1"
---

## Major Features ##
1. Now the translator prints the warning when the`<summary>` tag contains `<code>` or `<example>` in  the C# documentation comments. These situations cause mistakes in the translated C++ documentation.
1. An opportunity to include/exclude `[TestCase]` as well as `[Test]` is added.
1. The `IsUpper` method is added to the `Char` class. Method `IsLower` behavior is brought in line with .Net.
1. The nested classes in a translated code are now ordered the way they are in the C# source code.
1. The `ExtendLifetimeAsWeakPostponed` method is added to the `MemoryManagement` class. It creates a smart pointer using the aliasing constructor and copies `object1` and `object2` to the "proxy" objects holder. The method creates a pointer to `object1` and extends the lifetime of all objects to the lifetime of this pointer. The resulting pointer guarantees all parameters to remain alive even if it is the only pointer that keeps track of them.
1. Now the `libcodeporting.translator.cs2cpp.framework_appleclang.dylib` library is linked to the freetype and fontconfig libraries by the use of `@rpath` instead of the full path.

## Minor Fixes ##
1. The operator used to concatenate string and a bool value is added. The behavior of this concatenation is brought in line with .Net.

## Full List of Issues Covering all Changes in this Release ##

| Key          | Summary | Category |
|--------------| --- |----------|
|SLIDESCPP-3532|Add warning about incorrect XML tag inclusion|Task|
|SLIDESCPP-3534|Porter: The `<nunit_categories>` option does not affect tests marked with the `[TestCase]` attribute|Task|
|PDFCPP-1983|CsToCppPorter - incorrect order of static variables|Task|
|PDFCPP-1975|Fix the problem of "Full paths to third-party libraries" in the system library (MacOS,'libcodeporting.translator.cs2cpp.framework_appleclang.dylib').|Task|
|PDFCPP-2004|Specifications string operators for bool values|Task|
|WORDSCPP-1195|"'_T': macro redefinition" warning is thrown when "#include <tchar.h>" is used.|Bug|

## Public API and Backward Incompatible Changes ##
1. The `DynamicCast`, `DynamicCast_noexcept`, `StaticCast`, `StaticCast_noexcept` methods use `dynamic_cast` that looks incorrectly. These methods will be marked as deprecated and removed in the upcoming releases. The `ExplicitCast` and `AsCast` methods will be used instead of them. The translator will generate a code that uses the new casts.
1. The obsolete `_T(x)` macro is removed.
