---
date: "2019-10-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.10"
linktitle: "CodePorting.Native Cs2Cpp 19.10"
menu:
  docs:
    parent: "2019"
    weight: "11"
lastmod: "2019-10-11"
weight: "11"
---

## Major Features ##

1. No defines are needed any longer to be set up in the project that uses CodePorting.Native Cs2Cpp.
1. Visual Studio 2019 support was added.

## Minor fixes ##

1. PCH files were dropped from non-VS packages.
1. Windows DLLs are now all signed with the Aspose certificate.
1. The memory leak was fixed when working with regular expressions.
1. The release schedule is being adjusted.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
| CSPORTCPP-2906 | Improve release structure and procedure | Task |
| CSPORTCPP-2853 | Move default defines into library header | Task |
| CSPORTCPP-2909 | Sign DLLs | Enhancement |
| CSPORTCPP-2893 | Fix memory leak in asposecpplib | Bug |

## Public API and Backward Incompatible Changes ##

1. Previously, CMake scripts generated for ported code supplied some defines. These were also supplied from the Nuget package. Using these is not required any longer.
