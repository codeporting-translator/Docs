---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 18.12"
linktitle: "CodePorting.Native Cs2Cpp 18.12"
menu:
  docs:
    parent: "2018"
    weight: "5"    
lastmod: "2019-05-28"
categories:
- "release_notes_2018"
weight: "5"
---

## Major Features ##

1. original_tests_names option is added to disable prefixing tests names with categories names.
1. force_include_enum option is added to enabling adding '#include' directives for enums instead of generating forward declarations in all cases.

## Minor fixes. ##

1. Region::IsVisible(PointF) method is implemented.
1. Tests stub file (list of tests in ported project) is now sorted by test name.
1. The problem is fixed for 'finally' block operating on variables that have already been freed by 'return' statement (moving constructor optimization issue).
1. System::Convert::FromBase64String() now throws exception of correct FormatException type on input of unexpected format.
1. DateTime constructors now throw valid ArgumentOutOfRangeException exceptions on out-of-range dates or monthes being passed to them.
1. Callstack print of SmartPtr class destructor was ligtened by 1/3.
1. Messages of NotImplementedException, NotSupportedException and CultureNotFoundException are fixed to match .NET ones.
1. Order of constexpr fields in output files is fixed to avoid initialization fiasco.
1. A bug is fixed blocking some messages from reaching porter.log.
1. Messages for config errors now include file, line and position information.
1. A bug is fixed in porter generating 'gtest not found' errors if current directory was not 'bin/porter'.
1. TEST_F macros in output files are now placed near the test methods they call to make debug easier

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##


| Key | Summary | Category
---| ---|  ---|
|PDFCPP-817|Fix issues with OriginlKit functional tests|Task
|PDFCPP-834|Fix Region.IsVisible|Task
|CSPORTCPP-2295|Fix null reference after returning pointer used in 'finally' block|Bug
|WORDSCPP-702|System::Convert::FromBase64String throws an incorrect exception for invalid input|Bug
|WORDSCPP-696|System::DateTime implementation doesn't catch exceptions from Boost.DateTime|Bug
|CSPORTCPP-2302|Investigate reducing destructors call stack|Investigation
|WORDSCPP-676|Always add include directives for enums instead of forward declarations|Task
|WORDSCPP-360|Reorder constexpr field declaration|Task
|CSPORTCPP-2331|porter.log missing entries|Bug
|CSPORTCPP-2151|improve porter error messages on config issues|Task
|CSPORTCPP-2153|Porter not finding gtest if current directory is not asposecpplib/bin|Bug
|CSPORTCPP-2327|Attach TEST_F macros to corresponding test functions|Task
|WORDSCPP-683|Unexpected exception suffix message for NotSupportedException exception|Bug

## Public API and Backward Incompatible Changes ##

1. IsVisible(PointF) method is added into Region class
{{code language#"cpp"}}PointF point(22.5, 90.2);
bool visible # myRegion.IsVisible(point);```
