---
date: "2022-06-10"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 22.6"
linktitle: "CodePorting.Translator Cs2Cpp 22.6"
menu:
  docs:
    parent: "2022"
    weight: "7"
lastmod: "2022-06-10"
weight: "7"
---

## Major Features ##

1. The project was renamed to CodePorting.Translator Cs2Cpp. The 'CodePorting.Native Cs2Cpp' name is now obsolete. From now on, the Nuget packages are CodePorting.Translator.Cs2Cpp.Framework for the library and CodePorting.Translator.Cs2Cpp.Control to control the translator and the translated code. MSBuild and CMake targets are CodePorting.Translator.Cs2Cpp.Framework. Dynamic libraries' base name is codeporting.translator.cs2cpp.framework (e. g. codeporting.translator.cs2cpp.framework_vc14x86d.dll for 32-bit Debug Visual Studio version). Some APIs that contained product name were also changed to match the new naming scheme.
1. The implementations of StackArray and ArrayView classes were improved. Several library classes that work with arrays were extended to work with these array types as well.
1. The `force_static_cast` option was supported by the porter. It makes the ported code to use static_casts instead of dynamic_casts which can be useful if all casts are meant to succeed.
1. The Boost version used was updated to 1.79.0. The Botan version used was updated to 2.19.1. Some build issues with these libraries were fixed.

## Minor fixes ##

1. The CppArrayOnStack attribute can now be used with properties.
1. The invocations of parent indexer setter are now translated properly.
1. The code generated when the CppSkipTestDefinition attribute is placed at the test method was fixed.
1. Tha bug was fixed making a WeakReference class a value type in the translated code.
1. The signature of the `GetSharedMembers()` method was optimized in the library and translated code.
1. The translation of in-array delegate assignment was fixed.
1. Proper exceptions are now thrown from the Dictionary methods if nullptr is passed as a key.
1. The translator now supports using the inherited methods as TestCaseSource.
1. Doxygen version used to generate the documentation was upgraded.
1. The List::ForEach method now throws proper exceptions if the predicate changes the number of elements in the list.
1. The growth of big (up to ~2 billion elements) MemoryStream was fixed.
1. The DashCap::Triangle mode was supported. Other modes of the caps rendering were fixed.
1. The behavior of String::IndexOf was fixed for the empty string case.
1. The translation of Enum.TryParse calls were fixed for some parameter sets.
1. The Tag property of the Image class was supported.
1. Several Clang-Tidy warnings were fixed across the codebase, potentially improving the performance.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-5010 | Rename Nuget and downloadable packages and GitHub pages | Task |
| PDFCPP-1855 | Add specialization for StackArray<bool, N> | Task |
| PDFCPP-1859 | Add possibility to apply CppArrayOnStack attribute on properties (CsPorter) | Task |
| CSPORTCPP-5150 | Fix a compile error in ComplexClassBasePropertiesImpl | Bug |
| CSPORTCPP-5129 | Fix a compile error in SkipTestDefinition | Bug |
| CSPORTCPP-5181 | Remove WeakReference from force_value_types | Bug |
| TASKSCPP-1732 | Change GetSharedMembers semantic to improve leak-detection performance. | Task |
| SLIDESCPP-3459 | Porter: Incorrect translation of delegate assignment | Bug |
| CSPORTCPP-3448 | error: SEH exception with code 0xc0000005 thrown in the test body | Bug |
| CSPORTCPP-3446 | Invalid exceptions are thrown while passing null as the parameter value of a method | Bug |
| WORDSCPP-1177 | Refactoring of Unified tests broke C++ porting | Bug |
| PDFCPP-1879 | MemoryStream big size issue | Bug |
| SLIDESCPP-3452 | PDF import throws NotImplementedException error | Bug |
| CSPORTCPP-5211 | Support CodePorting.Native Cs2Cpp | Task |
| CSPORTCPP-4968 | Two different boost versions linked into asposecpp | Bug |
| PDFCPP-1896 | Fix String::IndexOf with StringComparison parameter | Bug |
| BARCODECPP-500 | Fix Enum.TryParse() translation | Bug |
| BARCODECPP-501 | Implement Image.Tag | New Feature |
| CSPORTCPP-5282 | Update Boost, Botan, Skia for all platforms | Task |

## Public API and Backward Incompatible Changes ##

1. The product was renamed. This includes Nuget package naming, DLL and lib files naming, lesser API changes, etc.
1. The supported glibc version will be changed in the upcoming releases.
