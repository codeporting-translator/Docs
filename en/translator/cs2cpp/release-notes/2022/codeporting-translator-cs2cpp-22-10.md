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

1. Now the translator prints the warning when the`<summary>` tag contains `<code>` or `<example>` in  the C# documentation comments. These situations cause mistakes in the translated C++ documentation.
1. Now the `nunit_categories` configuration file node can be used to filter tests marked by `[TestCase]` as well as `[Test]`.
1. The `IsUpper` method is added to the `Char` class. Method `IsLower` behavior is brought in line with .Net.
1. The nested classes in a translated code are now ordered the way they are in the C# source code.
1. The `ExtendLifetimeAsWeakPostponed` method is added to the `MemoryManagement` class. It creates a smart pointer using the aliasing constructor and copies `object1` and `object2` to the "proxy" objects holder. The method creates a pointer to `object1` and extends the lifetime of all objects to the lifetime of this pointer. The resulting pointer guarantees all parameters to remain alive even if it is the only pointer that keeps track of them.
1. Now the `libcodeporting.translator.cs2cpp.framework_appleclang.dylib` library is linked to the freetype and fontconfig libraries by the use of `@rpath` instead of the full path.
1. Now the `Image::get_Flags()` and `Bitmap::get_Flags()` methods support the `ImageFlags::HasRealDpi` flag.

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
