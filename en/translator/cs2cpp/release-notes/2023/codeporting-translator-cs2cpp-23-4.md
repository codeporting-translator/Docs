---
date: "2023-04-11"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.4"
linktitle: "CodePorting.Translator Cs2Cpp 23.4"
menu:
  docs:
    parent: "2023"
    weight: "1"
lastmod: "2023-04-11"
weight: "1"
---

## Major Features ##

1. Delegate behavior brought to a state closer to that in C#.
    * The delegate comparison logic has been redesigned. The system no longer tries to compare delegates written via `std::bind` or `std::function` by postulating that they are not equal.
    * Delegates as member functions are written without `std::bind`, which allows graceful detachment in accordance with similar behavior in C#.
    * Delegate now behaves like a full fledged immutable reference type.
    * Added `operator +` and `operator -`. The order of adding and removing callbacks has been corrected.
    * Lots of other tweaks.

## Minor fixes ##

1. Accessor by index in GroupCollection is overriden.
1. Name qualification for template base members is fixed.
1. Removed unnecessary `static_cast` generation when using trivial numeric types.
1. The `AllowBoxing` attribute is deprecated.
1. Fixed boxing when initializing static class fields.
1. Fixed ambiguity in getting types of boxed objects.
1. Removed circular dependency between `boxed_value.h` and `object_ext.h` files.
1. Zip stream is reset now to allow reading after an unsuccessful attempt.
1. Property overriding now checks handle when getter and setter declared in different interfaces.
1. Unnecessary `CppConstMethod` attribute for `ICollection` indexer is removed.
1. Fixed generation of parameterized boolean tests.
1. Added new feature to keep CRC stream position.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-5818 | Fix name qualification for template base members. | Bug  |
| CSPORTCPP-2133 | Improve casting translation | Enhancement  |
| SLIDESCPP-3705 | False triggering of "Property does not override" error | Task |
| CSPORTCPP-5827 | Remove unnecessary indexer attribute for ICollection indexer. | Investigation |
| SLIDESCPP-3708 | Fix RegressionTests_v23_4.SLIDESNET_43822 test | Task |
| TASKSCPP-1833 | Fix `[Values]` attribute porting for bool parameters | Task |

## Public API and Backward Incompatible Changes ##

None.
