---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.3"
linktitle: "CodePorting.Native Cs2Cpp 19.3"
menu:
  docs:
    parent: "2019"
    weight: "3"
lastmod: "2019-10-11"
weight: "3"
---

## Major Features ##

1. Visual Studio warnings were fixed in both ported code and library headers.
1. force_public_headers option was added which makes sure no private headers are included from public ones.
1. The header and source files which turn out empty due to entity skipping are no longer generated.
1. In Porter GUI, path to porter configuration was not updated when loading the workspace. This was now fixed.

## Minor fixes. ##

1. Missing calls to type conversion operators were fixed for static variables initializers.
1. String::Format() method now works with arrays of non-pointer types (e. g. strings) and with floating point types.
1. Error message when trying to build 32-bit code in VS against product Nuget package was fixed.
1. MD5CryptoServiceProvider class default constructor was implemented.
1. Exporting macro for DateTime::get_Today() was fixed.
1. cpp_enum_enable_metadata option was fixed to work for public enums only to avoid growing binary size too much.
1. export_internals option attribute was fixed. Previously, some internal classes were exported.
1. Explicit interface implementations now port as public which fixes some compilation issues.
1. Contexpr ordering was fixed for some types of expressions. Include generation for these was fixed as well.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|PDFCPP-911|CsToCppPorter - type conversion on static vaiable initialization|Task
|PDFCPP-912|asposecpplib - fix String.Format|Task
|CSPORTCPP-1663|Clean library warnings|Task
|CSPORTCPP-2515|Fix error message in NuGet package at building with 32bit compiler.|Task
|PDFCPP-916|Fix MD5CryptoServiceProvider|Task
|WORDSCPP-677|Move to the public includes any structs used as class fields (even private)|Bug
|WORDSCPP-765|Generating enum metadata for public enums only|Task
|PDFCPP-920|Fix String::Format for double and float|Task
|WORDSCPP-762|export_internals#true porter configuration option doesn't work as expected|Bug
|WORDSCPP-481|Incorrect porting of default access specifier of interface implementation|Bug
|WORDSCPP-763|Interface implementation ignores make_shared_lib/export_internals#true porter option
|WORDSCPP-360|Reorder constexpr field declaration.|Task
|WORDSCPP-745|Implement CppDelete attribute for excluding whole from porting|Task
|CSPORTCPP-2571|Fix workspace loading|Bug

## Public API and Backward Incompatible Changes ##

None.
