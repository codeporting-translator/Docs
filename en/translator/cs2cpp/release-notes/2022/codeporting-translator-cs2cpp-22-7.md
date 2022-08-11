---
date: "2022-07-12"
author:
  display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 22.7"
linktitle: "CodePorting.Translator Cs2Cpp 22.7"
menu:
  docs:
    parent: "2022"
    weight: "2"
lastmod: "2022-07-12"
weight: "2"
---

## Major Features ##

1. Translation of lambdas is improved. The `avoid_lambda_holders_if_possible` option is removed and the corresponding `pass_by_reference_when_holder_is_redundant` value is added to the `default_lambda_capture_mechanism` option.
1. The Skia version used is updated.
1. The libc version detection mechanism is fixed in the cmake scipts and now it works on RHEL.

## Minor Fixes ##

1. The redundant `static_cast` to a delegate is present in a delegate constructor in a translated code when a lambda captures variables using holders or captures by value. Fixed.
1. The `ParameterizedThreadStart`, `ThreadStart`, `WaitCallback`, `TimerCallback`, and `AsyncCallback` delegates are now aliases to `System::MulticastDelegate` instead of aliases to `std::function`.
1. `Bitmap::LockBits` works incorrect on the `kGray_8_SkColorType` images. Fixed. Now the `kGray_8_SkColorType` images are treated as `kN32_SkColorType`.
1. Performance of the `Bitmap::LockBits`, `Bitmap::GetSkBitmapFromArray` and `Bitmap::ConvertToARGBImage` methods is improved.
1. The 'System::SystemException: ucnv_fromUChars failed with error code 10' error is thrown when an emptry replacement string is passed to the `EncoderReplacementFallback` constructor. Fixed.
1. The result type of the coalesce function is fully aligned to the result type of the ?? operator in C#.
1. The kerning and ligature settings are improved.
1. The `return isnull ? nullptr : u"relax";` translated code calls an incorrect constructor. Fixed.


## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-5341 | Remove the 'avoid_lambda_holders_if_possible' option and add the corresponding value to 'default_lambda_capture_mechanism' | Task |
| CSPORTCPP-5295 | The redundant static_cast is present in the translated code | Bug |
| CSPORTCPP-5282 | Update Boost, Botan, Skia for all platforms| Bug |
| PDFCPP-1911 | Bitmap::LockBits works incorrect on kGray_8_SkColorType images | Task |
| PDFCPP-1922 | RegressionTests_v5_8.PDFKITNET_28367 fails with exception 'System::SystemException: ucnv_fromUChars failed with error code 10' | Task |
| TASKSCPP-1748 | Fix null-colescing in system library | Task |
| CSPORTCPP-4761 | Revise glibc we use | Task |
| SLIDESCPP-3414 | Fix RegressionTests_v22_3.SLIDESNET_35671 test | Task |
| PDFCPP-1930 | Issue with porting Null value strings in conditional expressions | Task |


## Public API and Backward Incompatible Changes ##

1. The supported glibc version remains the same.
