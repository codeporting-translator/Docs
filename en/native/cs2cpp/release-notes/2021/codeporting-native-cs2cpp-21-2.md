---
date: "2021-02-10"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.2"
linktitle: "CodePorting.Native Cs2Cpp 21.2"
menu:
  docs:
    parent: "2021"
    weight: "2"
lastmod: "2021-02-10"
weight: "2"
---

## Major Features ##

1. Now the porter can prolong the captured variables’ lifetime. A runtime error doesn't occur any longer when the delegate leaves the original scope it was created in. The 'default_lambda_capture_mechanism' and 'avoid_lambda_holders_if_possible' porter options were added to control this behavior. For fine tuning, one may use the 'CppLambdaPassByReference', 'CppLambdaPassByValue', 'CppLambdaShouldCaptureByReference' and 'CppLambdaUseHolder' attributes.
1. The porter is now translating a code from the documentation comments so that the ported documentation has a C++ look rather than a C# one. This behavior can be controlled by the 'translate_code_from_comments' and 'translate_code_from_comments_system_dlls' porter options.
1. The documentation for the library code was extended and improved.
1. Debugging helpers for Qt Creator were added to the release package.

## Minor fixes ##

1. The PrintTo method instantiation was fixed for some cases.
1. The 'forwarding_if_possible' porter option was fixed.
1. Some unwanted macros were removed and/or replaced with their direct equivalents both in the library and in the ported code.
1. Some issues with includes and forward declarations in the ported code were fixed. This includes their instability, missing includes, ignoring the CppForceInclude and CppForceForwardDeclaration attributes.
1. In some contexts, the porter no longer ignores the actual value of the 'endl' option.
1. The FastRTTI mode now works properly when there are no instantiatable classes present in the hierarchy.
1. The Path::GetDirectoryName() function now normalizes the path, same as in .Net.
1. The exception throwing was put in line with .Net implementation when items are added into XmlNamespaceManager.
1. Creation of bold and italic glyphs for the incomplete fonts was fixed on Linux.
1. Some potential memory leaks and crashes were fixed in the Thread class.
1. Widths of some synthesized glyphs were fixed.
1. The exceptions throwing when adding items into XmlNamespaceManager were put in line with .Net implementation.
1. More cmake target scripts were added into the release package to make sure that the Ninja build system works properly with the MSVC libraries.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-3997 | Check and fix release | Task |
| CSPORTCPP-1410 | Fix lambdas referring to out of scope stack variables | Bug |
| CSPORTCPP-4009 | Fix includes maze as provided by Aspose.Slides | Bug |
| SLIDESCPP-2715 | Porting code from a text description | New feature |
| SLIDESCPP-2722 | Fix FastRTTI compilation errors when no instanceable class is in a hierarchy | Bug |
| CSPORTCPP-2705 | Fix missing comments | Task |
| CSPORTCPP-4019 | Support CodePorting.Native Cs2Cpp | Task |
| SLIDESCPP-2241 | Fix TestThumbnailRendering.SLIDESNET_35509 test | Bug |
| SLIDESCPP-2757 | Improve System::IO::Directory error handling | Bug |
| SLIDESCPP-2620 | Сreate bold and italic styles for incomplete fonts (Linux) | Task |
| CSPORTCPP-3697 | Thread leaks memory | Bug |
| CSPORTCPP-4058 | Support CodePorting.Native Cs2Cpp | Task |
| SLIDESCPP-1977 | Fix text width when using bold font style where a font has only regular style | Bug |
| WORDSCPP-1057 | Write QtCreator debugging helpers for the most common used System:: types | New feature |
| WORDSCPP-1036 | Fix tests trown NullReferenceException. | Bug |
| CSPORTCPP-4040 | Add more targets scripts | Task |
| CSPORTCPP-4074 | Some CppForceInclude attributes get ignored | Bug |

## Public API and Backward Incompatible Changes ##

None.