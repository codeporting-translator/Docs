---
date: "2023-03-13"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.3"
linktitle: "CodePorting.Translator Cs2Cpp 23.3"
menu:
  docs:
    parent: "2023"
    weight: "9"
lastmod: "2023-03-13"
weight: "9"
---

## Major Features ##

1. The generation of indexer-setter methods has been reworked.
1. Updated the Doxygen documentation.

## Minor fixes ##

1. A typo was fixed in the `NullReferenceException` default error message.
1. Implemented the `Graphics::Flush` method.
1. Added exception support for Emscripten.
1. Accessor by index in `System::Text::RegularExpressions::GroupCollection` is overriden.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3578 | Improve usability of Aspose.Slides for C++ API for setter expressions | Enhancement |
| SLIDESCPP-3656 | Update the Doxygen configuration file | Task |
| SLIDESCPP-3686 | Fix a typo in the asposecpplib library | Bug |
| SLIDESCPP-3659 | Implement Graphics::Flush(FlushIntention intention) method | Task |
| PDFCPP-2210 | Build 'asposecpplib 23.2' on Emscripten | Task |

## Public API and Backward Incompatible Changes ##

None.
