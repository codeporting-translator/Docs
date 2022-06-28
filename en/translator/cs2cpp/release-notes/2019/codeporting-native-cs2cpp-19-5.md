---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.5"
linktitle: "CodePorting.Native Cs2Cpp 19.5"
menu:
  docs:
    parent: "2019"
    weight: "6"
lastmod: "2019-10-11"
weight: "6"
---

## Major Features ##

1. [generate_enum_descriptions](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/#Hgenerate_enum_descriptions) porter option was introduced.
1. TestCaseData NUnit class was supported by porter.

## Minor fixes ##

1. Bitwise operators were provided for some enums from the library.
1. Parsing of numeric enum values was supported by Enum::Parse() methods family.
1. Error was fixed when writing zero length string to XmlTextWriter.
1. Operator overload lookup was fixed in porter for some cases.
1. Conversion from user class to nullable wrapper was fixed in assignment operator.
1. XmlDocument::CreateAttribute was fixed for the case with XML namespace passed.
1. Remaining references to boost.regex were removed from the library.
1. Some code smell issues reported by static analyser were fixed.
1. Enum::GetName() was fixed for boxed flag enum values.
1. Some clang warnings were fixed in ported code and library headers.
1. Region::Intersect() method was fixed for the case of two similar regions.
1. OutOfMemory exception was fixed when using PathGradientBrush.
1. Assertion fault crash was fixed when drawing empty GraphicsPath with customized pen.
1. Some Skia internals were exposed for internal use only.
1. Porting of Assert.DoesNotThrow() was fixed.
1. Some cases of regex matching against empty string were fixed.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|PDFCPP-938|Port Aspose.Foundation.Tests project|Enhancement
|PDFCPP-954|Fix XmlDocument::CreateAttribute with xml namespace|Enhancement
|CSPORTCPP-2639|Remove unused boost.regex from cpplibs, asposecpplib and boost build scripts|Enhancement
|PDFCPP-953|Fix saving document in docx format|Enhancement
|CSPORTCPP-2451|Fix clang warnings|Enhancement
|SLIDESCPP-1694|Fix Region::Intersect operation of two similar regions|Enhancement
|SLIDESCPP-1695|Fix OutOfMemory exception when using PathGradientBrush|Enhancement
|WORDSCPP-638|Add support for porting TestCaseData|Enhancement
|PDFCPP-959|Fix drawing empty path with customized pen|Enhancement
|WORDSCPP-455|Manually implement Rendering PAL classes on Skia|Enhancement
|TASKSCPP-1121|Found error in macro ASPOSE_GTEST_TEST_THROW_ that cause compile error|Enhancement
|PDFCPP-965|fix asposecpplib regex matching empty string|Enhancement
|TASKSCPP-1108|Porter generate invalid code while casting user-defined class to nullable one|Bug

## Public API and Backward Incompatible Changes ##

1. Stub members were added into X509Certificate2 and ProcessStartInfo classes.
1. A stub was added for HttpWebResponse::GetResponseHeader method.
