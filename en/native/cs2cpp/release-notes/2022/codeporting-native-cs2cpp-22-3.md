---
date: "2022-03-05"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 22.3"
linktitle: "CodePorting.Native Cs2Cpp 22.3"
menu:
  docs:
    parent: "2022"
    weight: "1"
lastmod: "2022-03-05"
weight: "1"
---

## Major Features ##

1. The iterators provided by the IEnumerable class and the duck-typed collections were improved. The performance was raised for those implemented via the native C++ containers, and the assignment to the referenced element was supported where possible.
1. The new `build_cs_projects` option was supported by the porter.
1. The definitions of some attributes were fixed. The `AttributeUsage` mask was updated.
1. The method overloads were added to numerous classes that allow using `ArrayView` where only `ArrayPtr` was supported previously.
1. The default messages of the exception classes were put in line with the .Net behavior. No language-specific messages are supported at the moment.

## Minor fixes ##

1. The new constructors of the `System::Drawing::FontFamily` class were supported.
1. The visibility of the nested private classes was fixed.
1. The RUNPATH entry was deleted from the Linux and MacOS dynamic libraries.
1. The new `collect_porter_coverage_info` option was supported by the porter. However, it is only available to Aspose users.
1. The RIPEMD-160 hash algorithm was supported by the library.
1. On MacOS, the RTTI information for the exception types was properly exported. Now the exceptions thrown by CodePorting.Native Cs2Cpp or dependent projects can be caught by the customer's code.
1. The size of the CodePorting.Native Cs2Cpp library was reduced on MacOS and Linux.
1. The `operator ==` declared in the `System` namespace was causing compilation issues with the third party code if exported to the global namespace. This was fixed.
1. The compilation issues were fixed for the case of the direct inclusion of some headers.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3372 | Add MacOS support to the FontFamily class' methods for getting generic fonts | Task |
| SLIDESCPP-3364 | Porter: Fix the visibility of nested private classes | Bug |
| CSPORTCPP-4967 | Revert changes related to RUNPATH from asposecpplib | Bug |
| WORDSCPP-1169 | Implement RIPEMD160 hash algorithm | New feature |
| CSPORTCPP-3465 | Improve EnumeratorBasedIterator | Enhancement |
| CSPORTCPP-4756 | Fix regressions reported by product teams | Bug |
| CSPORTCPP-4931 | Make sure gold tests cs files are compilable | Enhancement |
| SLIDESCPP-3394 | Fix uncatched exceptions issue in Aspose.Slides for C++ for MacOS | Bug |
| SLIDESCPP-3393 | Reduce CodePorting.Native.Cs2Cpp library size in Linux and MacOS | Enhancement |
| PDFCPP-1730 | Add support ArrayView for other `System::*` classes | Enhancement |
| CSPORTCPP-3466 | An invalid message is generated if the default constructor is used | Bug |
| WORDSCPP-1170 | SmartPointer code is not checking is method IsNull exists | Bug |
| CSPORTCPP-4987 | Compile each library header individually | Enhancement |

## Public API and Backward Incompatible Changes ##

1. The iterator types of `System::Collections::Generic::IEnumerable` and duck typed-collections were completely reworked. This may alter some code.
1. The supported glibc version will be changed in the upcoming releases.
