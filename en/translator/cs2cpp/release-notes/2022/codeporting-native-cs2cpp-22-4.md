---
date: "2022-04-08"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 22.4"
linktitle: "CodePorting.Native Cs2Cpp 22.4"
menu:
  docs:
    parent: "2022"
    weight: "6"
lastmod: "2022-04-08"
weight: "8"
---

## Major Features ##

1. From the configuration file, the attributes can now be applied to static constructors and protected internal members.

## Minor fixes ##

1. The code generation was fixed in the porter for the case of assigning exception class field.
1. On MacOS, the BitMap class constructor no longer ignores the failure to allocate too large bitmap.
1. The `GetSharedMembers()` method is now const both in the library classes and in the generated code.
1. In some cases of translating delegates, the code generation was fixed to avoid missing the required forward declarations.
1. The `getInternalPtr()` method was supported by the `System::SmartPtr` class.
1. The porter was missing the global namespace references in the generated code for some cases of matching namespace names. This was fixed.
1. The `System::Drawing::GetThumbnailImage()` method was implemented.
1. The `RegionDataNodeRect` constructor was optimized.
1. The 1 bit per pixel black-and-white image loading was fixed.
1. The comparison of the infinite and empty regions was fixed.
1. The CppMemberwiseClone implementation was moved to the cpp files in the ported code.
1. The porting of the argumentless `Action` delegate was fixed.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-5032 | Fix Exception field assignment | Bug |
| SLIDESCPP-3417 | The Bitmap::Bitmap(width, height) constructor does not throw an exception in MacOS when it is expected | Bug |
| SLIDESCPP-3204 | Make GetSharedMembers method constant | Enhancement |
| CSPORTCPP-5044 | Fix ForwardDeclarationsSorting C++ compilation errors | Bug |
| PDFCPP-1806 | Fix porter bug with reading attribute CppArrayOnStack from the configs, assigned to static constructor | Bug |
| SLIDESCPP-3391 | Improve cycles detection mechanism logging | Enhancement |
| CSPORTCPP-5045 | Fix DoxygenTranslateCodeFromComments C++ compilation errors | Bug |
| PDFCPP-1795 | Implement Image::GetThumbnailImage | New feature |
| PDFCPP-1814 | Loading tif file issue (from support team) | Bug |
| SLIDESCPP-3437 | Fix System::Drawing::Region class equality comparison | Bug |
| WORDSCPP-1173 | Fix or disable failed ApiExamples tests | Bug |
| CSPORTCPP-5077 | Fix invalid translation of System.Action | Bug |
| CSPORTCPP-5084 | Support CodePorting.Native Cs2Cpp | Task |

## Public API and Backward Incompatible Changes ##

1. The deprecated `system/current_rettype.h` header was dropped.
1. The supported glibc version will be changed in the upcoming releases.
1. The product will be renamed in the next release. This includes Nuget package naming, DLL and lib files naming, lesser API changes, etc.
1. The implementation of the WeakRefence class will be changed in the next release.
