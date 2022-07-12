---
date: "2022-05-11"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 22.5"
linktitle: "CodePorting.Native Cs2Cpp 22.5"
menu:
  docs:
    parent: "2022"
    weight: "3"
lastmod: "2022-05-11"
weight: "3"
---

## Major Features ##

1. The WeakReference class was completely reworked. The template form was supported.

## Minor fixes ##

1. The license check issue was fixed. The porter's output on invalid license was fixed.
1. The 'NULL' token was added into the restricted lists. If used as an identifier, it is renamed by porter to avoid compilation issues.
1. The porter now generates neccessary casting when referencing the overloaded methods.
1. The 'is' operator is now ported properly for string and integer literals.
1. The operators for the System::Nullable class were reworked so they no longer break unrelated code. The unused assignment operator was dropped from the System::Random class.
1. The porter now generates the correct inclusion code for accessing the baseclass'es delegates.
1. The porter now generates the neccessary global scope references when translating the 'new' operators.
1. The translation of the 'string plus null' expression was fixed.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-5093 | Implement template class System::WeakReference<T> | Enhancement |
| CSPORTCPP-5098 | Support CodePorting.Native Cs2Cpp | Task |
| CSPORTCPP-5111 | Fix invalid variable name - NULL | Enhancement |
| CSPORTCPP-5124 | Add casting for an overloaded static event handler | Bug |
| CSPORTCPP-5057 | Fix "is" operator for literals | Bug |
| CSPORTCPP-5019 | Check if our operators potentially break any code | Investigation |
| PDFCPP-1854 | Casting to base class when referencing delegates | Task |
| CSPORTCPP-5149 | Fix a compile error in SameGlobalTypeMember | Bug |
| PDFCPP-1865 | Incorrect translation ("string" + null) expression | Bug |

## Public API and Backward Incompatible Changes ##

1. The supported glibc version will be changed in the upcoming releases.
1. The product will be renamed in the next release. This includes Nuget package naming, DLL and lib files naming, lesser API changes, etc.
