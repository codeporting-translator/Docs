---
date: "2021-02-10"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.3"
linktitle: "CodePorting.Native Cs2Cpp 21.3"
menu:
  docs:
    parent: "2021"
    weight: "6"
lastmod: "2021-02-10"
weight: "6"
---

## Major Features ##

1. Code documentation was improved significantly. Some previously undocumented members now have descriptions.
1. Translation of type references in code comments was improved. Using 'translate_code_from_comments_system_dlls' configuration item is no longer required as the porter automatically looks through the types in all referenced assemblies. Class properties are now properly replaced when translating documentation comments.
1. Assert.That(Is.AtLeast) and Assert.That(Is.GreaterThanOrEqualTo) assertions were supported by porter when translating NUnit/xUnit tests.
1. SDK-styled csproj files were supported using Buildalyzer library. To use it, one must set the new 'use_buildalyzer' option.

## Minor fixes ##

1. Memory consumption when creating delegates was improved.
1. The 'CppDeclareFriendFunction' attribute is now supported by porter. It allows it generating friend declarations for the external functions.
1. Porter performance issue generating includes for ported files was fixed.
1. GetSharedMembers() virtual function trunk was excluded from both the library and the ported code.
1. The FINAL and OVERRIDE macros were retired.
1. An issue was fixed in porter when translating multiple TestCase-driven tests with same names belonging to different classes.
1. The rendering of synthesized font styles was improved to better match .Net behavior. Some issues rendering and measuring text were resolved.
1. HttpWebRequest class now properly handles relative pathes in redirect header.
1. The signature and contents of SetTemplateWeakPtr method was put in line with the rest of the code in both the library and the generated code.
1. Some issues translating the code which depends on Zip subsection of the Library were resolved.
1. The size of System::Object class instance was optimized.
1. The behavior of System::Threading::Thread::get_ManagedThreadId() was improved to guarantee unical identifiers for the threads.
1. Translation of foreach-based iteration on 'this' no longer causes compilation errors in const methods if 'IterateOver'-like approach is used.
1. The default constructors of ShaXXXManaged classes no longer throw NotImplemented exceptions.
1. The new 'always_include_delegates' option is supported by porter to include delegates definitions instead of redeclaring them each time.
1. System::IO::Compression::BrotliStream class was supported using Brotli library.
1. Missing include for System::ComponentModel::AsyncCompletedEventArgs in ported code was fixed.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-2705 | Fix missing comments | Bug |
| CSPORTCPP-4084 | High memory usage when creating delegates | Enhancement |
| WORDSCPP-1058 | Implement public Venture License API | Task |
| CSPORTCPP-4074 | Some CppForceInclude attributes get ignored | Bug |
| SLIDESCPP-2740 | Improve system types handling when porting comments | Enhancement |
| CSPORTCPP-4088 | Adding the new directive that is used to disable GetSharedMembers. | Task |
| CSPORTCPP-1922 | Remove some private headers from include directory and perform limited clean-up of public headers | Task |
| WORDSCPP-1060 | Support of Assert.That(..., Is.AtLeast(...)) expressions | Task |
| CSPORTCPP-3104 | Fix net namespace comments | Task |
| WORDSCPP-1061 | Fix porting of parameterized tests with the same name | Bug |
| SLIDESCPP-2803 | Check Skia with SK_USE_FREETYPE_EMBOLDEN define enabled | Task |
| CSPORTCPP-4036 | Update SetTemplateWeakPtr method generation | Enhancement |
| SLIDESCPP-2808 | Translate class members when porting comments | Task |
| SLIDESCPP-2689 | Use Aspose.Slides for .NET 21.3 features | Enhancement |
| SLIDESCPP-2789 | Fix text width in GraphicsTest.MeasureDrawString_06 | Task |
| SLIDESCPP-2790 | Fix text width in DrawStringTests.Test_10_* test | Task |
| CSPORTCPP-4092 | Remove redundant field m_globalMutex from System::Object | Task |
| EMAILCPP-253 | Improving performance of the ported code (investigating bottlenecks) | Task |
| EMAILCPP-282 | Fixing regressions in porter caused by 'Fewer includes' MR | Task |
| PDFCPP-1540 | Fix Threading::GetCurrentThread, Thread::ManagedThreadId | Task |
| PDFCPP-1538 | Fix enumerator_adapter to iterate over const pointers | Task |
| TASKSCPP-1567 | Enable managed SHA classes instantiation | Task |
| CSPORTCPP-4137 | Add 'always include delegates' option | Task |
| CSPORTCPP-4118 | Merge SDK-styled projects support | Task |
| EMAILCPP-296 | Prepearing Aspose.Email for C++ Release 21.2 | Task |

## Public API and Backward Incompatible Changes ##

1. .Net Framework used by porter was upgraded to 4.7.2 (was 4.6).
