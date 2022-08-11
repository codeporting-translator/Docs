---
date: "2022-02-09"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 22.2"
linktitle: "CodePorting.Native Cs2Cpp 22.2"
menu:
  docs:
    parent: "2022"
    weight: "7"
lastmod: "2022-02-09"
weight: "7"
---

## Major Features ##

1. The conflict was resolved when using both Aspose.Cells for C++ and CodePorting.Native.Cs2Cpp.API headers from the same file.
1. MacOS-specific release package was added.

## Minor fixes ##

1. The porter generated an incorrect code for creating instances of generic structures. This was fixed.
1. The look of API reference for `System::String::operator + ()` was improved.
1. The iostream-based overloads of constructors with Stream-based arguments were moved to ported cpp files where possible. Previously, it was neccessary for any user of such constructors to have all field types defined, which resulted in compilation errors.
1. Previously, applying the CppSkipDefinition attribute to a test method resulted to exclusing both the test method and the test definition (TEST_F) from the ported code. This made this attribute impossible to be used for replacing the test method definition with custom one while keeping the test itself. The test definition is no longer skipped by this attribute.
1. The new overload of the `Char::IsWhiteSpace()` method was added.
1. The porter now generates INT64_C/UINT64_C wrappers for constants that are not properly cast to int64_t/uint64_t by Apple Clang 13.0.
1. Misprint was fixed in the readme files from our release packages.
1. The System::WeakReference class was improved. Now it inherits the System::Object. Another constructor and RTTI information were added, the return type of the get_Target() method was fixed.
1. The glibc version detection in the cmake code was fixed for some platforms.
1. The rpath value was added to the Linux libraries.
1. The `PathGradientBrush::get_InterpolationColors()` method was fixed for some brushes.
1. The symbols from the Microsoft namespace are now properly exported on MacOS.
1. The behavior of the `Graphics::DrawPolygon()` method for incomplete polygons was fixed to match .Net behavior.
1. The version information was included into the library binaries.
1. The constructors of the Regex-related classes were sped-up by using shared pointer references instead of copying them.
1. The lambda variable capturing was working incorreclty in the porter if the lambda was used as an argument to a constructor call. This was fixed.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3292 | Porter: Incorrect generic structures translation | Bug |
| SLIDESCPP-3243 | Prepare C++ code examples (v22.1) | Task |
| WORDSCPP-1149 | std::stream to System::IO::Stream wrapped ctor doesn't compile on customer size | Bug |
| CSPORTCPP-4830 | NuGet packages Aspose.Cells.Cpp and CodePorting.Native.Cs2Cpp.API cannot be used together | Bug |
| SLIDESCPP-3359 | Implement the missing Char::IsWhiteSpace() method | New feature |
| CSPORTCPP-4864 | Add MacOS builds | New feature |
| CSPORTCPP-4904 | Support CodePorting.Native Cs2Cpp | Task |
| PDFCPP-1785 | Fix System::WeakReference class | Bug |
| WORDSCPP-1164 | Broken compiler detection | Bug |
| WORDSCPP-1163 | Possibility of adding -rpath='$ORIGIN/../lib' | New feature |
| PDFCPP-1790 | Fix PathGradientBrush | Bug |
| SLIDESCPP-3380 | Fix RegressionTests_v22_2.SLIDESJAVA_37693 test | Bug |
| CSPORTCPP-4945 | Lambda capture holders are missing | Bug |
| WORDSCPP-1145 | Port performance tests | Task |
| WORDSCPP-1157 | Too slow TestTxt.TestJira17890 | Enhancement |

## Public API and Backward Incompatible Changes ##

1. Some pointer type declarations (e. g. `StreamPtr`) were moved from the global namespace to 'System' namespace.
1. The supported glibc version will be changed in the upcoming releases.
1. The new `IEnumerable`-level iterators and iterators for collections with duck typing will be introduced in one of the upcoming releases. The code which is dependent on `EnumeratorBasedIterator` or `DuckTypedIterator` may require some changes.
