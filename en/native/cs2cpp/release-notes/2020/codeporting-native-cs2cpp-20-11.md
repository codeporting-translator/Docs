---
date: "2020-08-16"
author:
  display_name: "Muhammad Rizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.11"
linktitle: "CodePorting.Native Cs2Cpp 20.11"
menu:
  docs:
    parent: "2020"
    weight: "2"
lastmod: "2020-10-12"
weight: "2"
---

## Major Features ##

1. Number formatting was improved significantly and put in line with .Net behavior.
2. A new CppOverrideAccessModifiers attribute was supported making it possible to change access modifiers for types and entities in ported code.
3. GetSharedMembers() method generation can now be disabled by using a new generate\_get\_shared\_members option.
4. A bug was fixed in ported code allowing it to use private constructors via System::MakeObject() calls. Now, only those constructors available to calling context can be used.
5. In-type RTTI macros generation can now be disabled in porter by using a new generate\_rtti\_info option.
6. A new CppInline attribute was added to make it possible moving entities implementations into headers.
7. A new allow\_using\_directives\_in\_headers option was added to make porter generate shorter code by the price of adding using namespace directives into header code.
8. Porter now translates extension method calls into proper static member function calls. A new extensions\_as\_method option was added to make it possible switching to old behavior where needed.
9. It is now possible to control the order of types and members in output header file. To do so, one may use new CppPlaceAfter and CppPlaceBefore attributes.

## Minor Fixes ##

1. Many issues were fixed within System::Xml subsystem.
2. Redirects are now supported by HttpWebRequest implementation.
3. Creating a font with a new style now works properly on Linux.
4. Decimal::PrintTo() now always uses dot as a floating point delimiter.
5. Some components of object creation controlling system were moved to a different point in code.
6. Anti aliasing was fixed for some fonts rendering on Linux.
7. Loading fonts from byte arrays was fixed on Linux.
8. A new for\_each\_member\_cycles\_only porter option was added making it possible to filter output gv files so that they only contain reference loops.
9. The sorting order of the items returned by FontFamily::get\_Families now matches such in .Net.
10. A bug was fixed in List class causing resetting contained pointers modes to default on underlying vector resize.
11. Incorrect text cropping was fixed for EMF images.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

|   Key  |   Summary |   Category |
| --- | --- | --- |
| WORDSCPP-1016 | Fix formatting issues | Bug |
| CSPORTCPP-3641 | Porting tests for the System.XML namespace | Task |
| WORDSCPP-1021 | Support HttpWebRequest.AllowAutoRedirect property | New feature |
| SLIDESCPP-2599 | Fix creating a font with a new style (Linux) | Bug |
| SLIDESCPP-2586 | Porter: Implement mechanism that allows public classes be inherited from internal interfaces | New feature |
| WORDSCPP-1009 | Port Console.Write() and Console.WriteLine() calls using standart C++ stream-based I/O | Mew feature |
| CSPORTCPP-1049 | Move constructor self references and leakage detection to MakeObject() level | Enhancement |
| WORDSCPP-1023 | Disable GetSharedMembers() generation | New feature |
| CSPORTCPP-1387 | Fix all constructors being effectively public | Bug |
| WORDSCPP-1025 | Disable RTTI generation | New feature |
| SLIDESCPP-2572 | Fix RegressionTests\_v16\_5.SLIDESJAVA\_35418 text (Linux) | Bug |
| SLIDESCPP-2616 | Fix using fonts from streams in Linux | Bug |
| WORDSCPP-1026 | Add porter attribute CppInlineAttribute | New feature |
| WORDSCPP-1028 | Use &quot;using namespace NNN;&quot; expressions in header files | New feature |
| WORDSCPP-1024 | Add support for general extension methods | New feature |
| SLIDESCPP-2609 | Fix RegressionTests\_v20\_9.SLIDESNET\_39715 test | Bug |
| WORDSCPP-1030 | Add porter attribute CppDisableAutoReorderingAttribute | New feature |
| WORDSCPP-1031 | Add porter attribute to change class/struct/enum order in header files | New feature |
| PDFCPP-1453 | Environment::get\_WorkingSet implementation | New feature |
| WORDSCPP-1033 | Add porter attributes to change members order within a class | New feature |
| EMAILCPP-256 | HttpClientHandler Timeout error not detected | Bug |
| CSPORTCPP-3853 | Fix defaulting SmartPtr mode on container resize | Bug |
| SLIDESCPP-2637 | Incorrect text cropping on the EMF image | Bug |

## Public API and Backward Incompatible Changes ##

1. When instantiating ported classes using non-public constructors, member function MakeObject with corresponding parameters should be used instead of System::MakeObject().
2. A stub for System::Security::Cryptography::RandomNumberGenerator::Create() was added.
3. Environment::get\_WorkingSet() method was implemented, and its signature was brought in line with .Net version.
