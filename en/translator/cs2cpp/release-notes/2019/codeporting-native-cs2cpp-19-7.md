---
date: "2019-10-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.7"
linktitle: "CodePorting.Native Cs2Cpp 19.7"
menu:
  docs:
    parent: "2019"
    weight: "8"
lastmod: "2019-10-11"
weight: "8"
---

## Major Features ##

1. System::Globalization subsystem was reworked for better match to .NET implementation.
1. Multiple previously unsupported encodings were added into System::Text submodule.
1. ValueSource NUnit attribute was supported by porter.

## Minor fixes ##

1. The Initializer list for user IList-implementing classes were supported in porter.
1. Test methods alignment issues were fixed in ported code.
1. A regression was fixed which was introduced in 19.6 version which broke porting of initializer lists with braces inside them.
1. '[generatedlist_template](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/#Hgeneratedlist_template)' porter option was supported by porter making it possible to enlist generated files.
1. Most VS warnings of /W4 level were fixed or suppressed for both library and ported code.
1. Imaging::PixelFormat::Format32bppPArgb value was supported by multiple drawing algorithms.
1. Porting of IntPtr initializers was fixed.
1. System::Compare() method was fixed for NaN values.
1. Behavior of Fix System::Decimal::Round() was brought in line with .Net one.
1. Decimal behavior for courner cases (e. g. porting direct initialization) was fixed.
1. System::BitConverter::Int64BitsToDouble() method was supported.
1. System::Threading::ThreadInterruptedException class was supported.
1. Metadata was added for System::Drawing::Imaging::PixelFormat enum.
1. Assert.Inconclusive NUnit method was supported by porter.
1. Some headers unrelated to .Net classes were dropped from package.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
| WORDSCPP-787 | Add missing encodings to ICU and asposecpplib | Task
| TASKSCPP-1237 | Implement NUnits's Assert.Inconclusive method | Task
|TASKSCPP-1142 | Implement ValueSource source at parametrized NUnit test fixtures | Task
| CSPORTCPP-2729 | Support CsPorter for C++ | Task |
| WORDSCPP-664 | Rework System::Globalization subsystem | Task |
| PDFCPP-1005 | Introduce PixelFormat::Format32bppPArgb | Task |
| PDFCPP-1004 | Fix CsToCppPorter IntPtr with initializer | Task |
| CSPORTCPP-2018 | Get rid of 'system on zip' dependency | Task
| TASKSCPP-1189 | Ported from NUnit google-tests has wrongly aligned bodies, that makes ported code uncomfortable for reading. | Enhancement
| CSPORTCPP-2554 | Raise VS warning level | Enhancement
| TASKSCPP-1197 | Fix tests with failures on Decimal's accuracy | Bug
| TASKSCPP-1214 | Fix System::Decimal::Round(...) incorrect behaviour| Bug
| WORDSCPP-799 | TestCustomersCharts.TestJira12406 failed with STL assertion "invalid comparator" | Bug
| TASKSCPP-1188 | Porter quietly ignores initializer list for IList implementors. | Bug

## Public API and Backward Incompatible Changes ##

1. Classes inside System::Globalization namespace was reworked. Implementation-specific details not matching The .NET public API may have been removed.
1. include/zip headers were dropped as they are not required for ported code to work.
