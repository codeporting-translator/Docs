---
date: "2019-10-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.9"
linktitle: "CodePorting.Native Cs2Cpp 19.9"
menu:
  docs:
    parent: "2019"
    weight: "10"
lastmod: "2019-09-30"
weight: "10"
---

## Major Features ##

1. The release structure is being changed. More packages with a library built for specific compilers were added. This is being done to make it possible using multiple Aspose C++ projects in a single user application.
1. '[generate_includes_subdirectory](https://wiki.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/#Hgenerate_includes_subdirectory)' porter option was supported.
1. System::String:: Format() was fixed to work properly with boxed values.

## Minor fixes ##

1. Stubs for HMACSHA512 were added.
1. 'Assert.Zero()' NUnit method was supported.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
| CSPORTCPP-2771 | Support multiple C++ products in the single-user application | Task |
| PDFCPP-1057 | Implement stub for HMACSHA512 class | Task |
| CSPORTCPP-2863 | Resolve inclusion issues | Task |
| WORDSCPP-820 | System::String::Format incorrectly works with boxed values | Bug |

## Public API and Backward Incompatible Changes ##

1. Clang's libraries suffix was changed: version number and C++ library information was added into it.
