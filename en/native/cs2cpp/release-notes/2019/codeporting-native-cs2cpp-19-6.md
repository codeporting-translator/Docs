---
date: "2019-10-11"
author:
  display_name: "tilalahmad"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.6 "
linktitle: "CodePorting.Native Cs2Cpp 19.6 "
menu:
  docs:
    parent: "2019"
    weight: "7"
lastmod: "2019-10-11"
weight: "7"
---

## Major Features ##

1. C++ [code injection](https://wiki.codeporting.com/native/cs2cpp/developer-guide/limitations-and-bugs/cpp-code-injection/) feature is added which allows injection C++ code through C# code comments.
1. Remaining warnings produced by library headers and ported code are fixed for clang and GCC compilers.
1. Legacy 'csPorter.Cpp.API' and 'CodePorting.Native.CsToCpp' Nuget packages are dropped. No new versions of these will be planned.

## Minor fixes ##

1. Floating point numbers rounding issues are fixed for some cases of values rendering.
1. Missing 'override' keywords are placed around in library headers.
1. CollectionAssert.AreEqual and CollectionAssert.AreNotEqual methods are suppported when porting NUnit tests.
1. 'gb2312' codepage decoding is fixed.
1. Issue is fixed with weak pointers' assignment operator if the object being assigned to it has same address that the previous (deleted) one had.
1. Translation of structure initialization with list of named expressions is fixed.
1. FontFamily lookup is made case-insensitive, same as in .Net.
1. Porter-generated comments are fixed when translating 'using' statement.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
| CSPORTCPP-2328 | Clean warnings from porter's code | Task |
| CSPORTCPP-2450 | Fix GCC warnings | Task |
| CSPORTCPP-2451 | Fix clang warnings | Task |
| CSPORTCPP-2674 | Publish release into Nuget and Downloads | Task |
| CSPORTCPP-2721 | Support CsPorter for C++ | Task |
| PDFCPP-608 | Investigate issue with Blender class performance | Task |
| TASKSCPP-1141 | Implement CollectionAssert.AreEqual method | Task |
| WORDSCPP-239 | Difference in ICU and .Net when encoding invalid byte sequence | Task |
| CSPORTCPP-2496 | Porter: Wrong translation of an object-initialized structure with arguments | Bug |
| TASKSCPP-1163 | All tests from TestViewsRendering.* failed | Bug |
| WORDSCPP-695 | Incorrect Encoding for 936 code page | Bug |

## Public API and Backward Incompatible Changes ##

1. Several stubs and implementations are added for System::Drawing::Printing namespace members.
