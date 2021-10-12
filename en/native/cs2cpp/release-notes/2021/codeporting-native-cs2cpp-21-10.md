---
date: "2021-10-11"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.10"
linktitle: "CodePorting.Native Cs2Cpp 21.10"
menu:
  docs:
    parent: "2021"
    weight: "1"
lastmod: "2021-10-11"
weight: "1"
---

## Major Features ##

1. Calls to the methods of System.Diagnostics.Debug are now translated into macros. This allows not performing any calculations in the Debug mode, same as in C#. The ‘alternative_debug_class’ option was supported by the porter. This option can be used to apply this behavior to other classes.
1. Debug libraries for Linux OS are no longer included in the CodePorting.Native Cs2Cpp packages. From now on, Release CodePorting.Native Cs2Cpp Linux libraries should be linked to your code both in Debug and in Release. For CMake users, no changes are required, as the references to correct libraries will be propagated automatically. In the pre-generated build and project files on Linux OS, references to libaspose_cpp_clang3_libstdcppd.so should be replaced with libaspose_cpp_clang3_libstdcpp.so, and references to libaspose_cpp_gcc6d.so should be repalced with libaspose_cpp_gcc6.so. For Windows users, no changes are required, as both Debug and Release libraries are still supplied for VS compiler.
1. API reference was improved for multiple classes. Usage examples were added. Some misprints were fixed.
1. The 'CppOverrideAccessModifier' attribute was improved. Protected and private modes were supported, and all analysis done by the porter is now affected by this attribute.

## Minor fixes ##

1. The Encoding::GetEncoding() method now throws ArgumentException if the encoding name is incorrect, same way as .Net implementation does.
1. The memory corruption issue was fixed within the System::String::Join method.
1. The methods of the MemoryManagement class were fixed to work properly with the arguments of the WeakPtr type.
1. The 'override' qualifiers are now properly placed when the CppRenameEntity attribute is used.
1. The bug was fixed in the porter which caused the 'CppOverrideAccessModifier' attribute place 'override' qualifier on the attributed methods.
1. Now the '\[\]' suffix can be used for the arrays in the in-config attributes.
1. The bug was fixed in the porter leaving some cases of ArrayRawPointer and ConstArrayRawPointer parameter modes unrecognized in some cases.
1. The bug was fixed in the porter affecting the 'import' configuration file tag sanity check.
1. Some previously missing options were added into default porter.config file.
1. The CppIgnoreBaseType attribute didn't erase the related include for base class. This was fixed in the porter.
1. The porter didn't auto-place friend declarations when porting the classes from the System namespace. This was fixed.
1. Some entities from non-generic System.Collections interfaces caused the porter to misplace 'override' qualifier in the classes that inherit these interfaces. This was fixed.
1. Some code was moved between the porter's assembilies. Some unrequired classes were removed.
1. The code generated for GetSharedMembers() method was fixed.
1. The DeflateStream class was fixed for having more than one entries being read simultaneously.
1. The porter didn't recognize and merge partial class declarations if non-partial types were declared in the same files. This was fixed.
1. The System::String::StartsWith method was fixed for the strings with BOM in them.
1. The Dns::GetHostByName method now throws proper SocketException on socket errors in non-English-cultured environments.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-4518 | StringTestCase::JoinObjectsTest system test is failed periodically | Bug |
| SLIDESCPP-3095 | Rework Debug.Assert translation to C++ | Enhancement |
| CSPORTCPP-2211 | Drop debug library from Linux releases | Task |
| CSPORTCPP-4561 | Fix MemoryManagement class to work with WeakPtr arguments | Bug |
| CSPORTCPP-4559 | Incorrect 'override' specifier placement | Bug |
| CSPORTCPP-4563 | Improve MemoryManagement class documentation | Enhancement |
| WORDSCPP-680 | Add an option for porting Aspose.Debug methods as macro | Task |
| SLIDESCPP-3127 | Porter: Improve CppOverrideAccessModifier attribute | Enhancement |
| CSPORTCPP-4568 | Support CodePorting.Native Cs2Cpp | Task |
| CSPORTCPP-4596 | Fix check of config attribute | Bug |
| CSPORTCPP-4576 | Update options default values | Enhancement |
| SLIDESCPP-3133 | Porter: Generation of an unnecessary #include directive | Bug |
| SLIDESCPP-3140 | Porter: Improve Debug.Assert translation when porting types from the System namespace | Bug |
| SLIDESCPP-2998 | Porter: Fix adding friend declarations for classes from System namespace | Bug |
| CSPORTCPP-4578 | Misplaced override in ported code | Bug |
| CSPORTCPP-4610 | Remove IBaseAttributeCondition | Task |
| CSPORTCPP-4611 | Refactor NRefactoryCollection | Task |
| CSPORTCPP-4605 | Support CodePorting.Native Cs2Cpp | Task |
| CSPORTCPP-4606 | Tests fail with ENABLE_CYCLES_DETECTION_EXT option enabled | Bug |
| SLIDESCPP-3148 | Fix RegressionTests_v21_10.SLIDESNET_42700 test | Bug |
| PDFCPP-1687 | Partial classes porting | Bug |
| SLIDESCPP-3159 | Fix System::String::StartsWith method behavior for strings with BOM | Bug |
| CSPORTCPP-3867 | Add usage examples for system classes | Task |
| CSPORTCPP-4635 | The 'DnsTest.TestExceptionType' test fails | Bug |

## Public API and Backward Incompatible Changes ##

1. libaspose_cpp_clang3_libstdcppd.so and libaspose_cpp_gcc6d.so are no longer populated. If you have references to these libraries in your project or build files, please update them to use libaspose_cpp_clang3_libstdcpp.so or libaspose_cpp_gcc6.so instead.
1. The CppAddFunctionArgument and CppPassFunctionArgument attributes are now deprecated. The ArrayRawPointer and ConstArrayRawPointer values of the CppArgumentKind attribute are also deprecated. The support of these will be removed in the next version of CodePorting.Native Cs2Cpp.