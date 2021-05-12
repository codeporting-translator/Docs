---
date: "2021-05-11"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.5"
linktitle: "CodePorting.Native Cs2Cpp 21.5"
menu:
  docs:
    parent: "2021"
    weight: "1"
lastmod: "2021-05-11"
weight: "1"
---

## Major Features ##

1. Project documentation on docs.codeporting.com was actualized and improved.
1. `OrderByDescending()` LINQ call was supported.

## Minor fixes ##

1. The code generated for the 'switch' statement was simplified.
1. The structure of target cmake files included into release packages was optimized.
1. `Path::GetRandomFileName()` method returning same names on consequent calls was fixed.
1. `StreamWriter` class incorrectly disposing inner stream in the destructor was fixed.
1. Porter no longer generates unneeded default constructors for exception classes.
1. Performance of BinaryReader class was improved.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| WORDSCPP-1072 | Simplify switch statements generated by porter | Enhancement |
| CSPORTCPP-4199 | Drop unwanted cmake files from release package | Task |
| BARCODECPP-451 | ArgumentException on saving SVG file | Bug |
| CSPORTCPP-4232 | Check if exceptions still need default constructors | Bug |
| CSPORTCPP-3947 | Improve public documentation support experience | Enhancement |
| SLIDESCPP-2890 | Improve BinaryReader class performance | Enhancement |
| EMAILCPP-298 | Prepearing Aspose.Email for C++ Release 21.4 | Task |

## Public API and Backward Incompatible Changes ##

None.