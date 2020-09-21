---
date: "2020-08-16"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.8"
linktitle: "CodePorting.Native Cs2Cpp 20.8"
menu:
  docs:
    parent: "2020"
    weight: "8"
lastmod: "2020-08-16"
weight: "2"
---

## Major Features ##

1. The behavior of many library classes was put closer to such of .Net originals in cases of exception throwing, parameter checking, and other details not concerning major features. This includes:
11. Exception classes;
11. Regular exceptions-related classes;
11. Multithreading-related classes;
11. Text and encoding-related classes;
11. Collection classes;
11. DateTime-related classes;
11. Some other classes.
1. Porter now supports the porting of an 'unsafe' block statement.
1. Porter is now capable of placing 'override' instructions in ported code where necessary.
1. CppConstWrapper attribute is now working properly for methods with arguments, non-virtual members, properties, and indexers.
1. Porter is now capable of generating more simplistic code for some cases:
11. 'simplify_using_statements' option was supported by porter making it possible to generate simpler code for 'using' statements which should be useful e. g. for examples porting.
11. Translation of Assert.Zero and Assert.AreNotEqual was simplified.
11. Translation of property initializers for variable creation statements was simplified.
11. Simplier code is now generated for 'foreach' statement translation if 'foreach_as_range_based_for_loop' option is enabled. This applies to library classes with 'begin-end' semantics support and to other classes specified via 'types_with_begin_and_end_methods' configuration file node.
1. Reflection support was extended. Many related classes were implemented. Porter is now capable of propagating attributes and constructors info (see 'attributes_into_reflection_info' option).

## Minor fixes ##

1. A simple implementation for sorting strings using CompareOptions::StringSort was added.
1. False positives for 'virtual function call from constructor/destructor' warning were fixed.
1. 'goto case' statement was fixed with the switch of character type.
1. XmlWriter's indentation settings were put in line with such in .Net.
1. A crash was fixed in ported code on some compilers caused by an exception being thrown from 'finally' block.
1. XmlConvert::TryDecodingName was fixed for non-ASCII characters.
1. Iterator comparison for System::Collections::ObjectModel::Collection was fixed.
1. Porter now properly replaces multiple documentation tags with the same name. The replacement of the 'exception' documentation tag was supported. Text for 'see' documentation tag now gets reduced for compact view.
1. StopWatch class was fixed to provide interval values during the counting, not only after it is stopped.
1. UTF-7 decoding is now fully supported.
1. Comment delimiters (bars of slashes) are now ported unmodified, no longer being recognized as triple-slash documentation comment prefix followed by a comment.
1. Dictionary class now works properly with structures implementing the IEquatable interface.
1. The shared pointer leak was fixed on the unsuccessful call of the dynamic_pointer_cast() method.
1. Compilation issues with Matrix class used without Point structure include were fixed.
1. Porter is now capable of generating platform-independent XXX_SHARED_CLASS macros for exporting classes.
1. Output file name unicalization can now be disabled using 'make_cpp_file_name_uniq' option.
1. Porter is now capable of translating casts to non-generic IList interface (see 'allow_cast_to_non_generic_list' option).
1. Previously unsupported types of tiff files were supported by the Drawing subsystem.
1. CppUnknownTypeParam, CppValueTypeParam, CppForceDynamicCastToTypeParam and CppForceDynamicCastFromTypeParam attributes can now be used from configuration files.
1. Some memory leaks were fixed in System::Xml submodule.
1. Figure connection was fixed in GraphicsPath::AddPath() method.
1. A shared pointer leak via creating an aliased null-pointer was fixed.
1. Equals() call via aliased pointer created based on weak pointer was fixed.
1. do_try_finally.h now works properly in C++17 mode.
1. Some world matrix-related drawing issues were fixed.
1. The vertical positioning of a fallback font was fixed.
1. Misprint in namespace end comments in ported code was fixed.
1. 'First' and 'Count' LINQ methods were supported in ported code.
1. XPathDocument(System.IO.Stream) constructor was supported.
1. Porter is now capable of removing comments to private entities (see 'remove_private_comments' option).
1. Codepage issues in license text were fixed.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|CSPORTCPP-1107|Add porter option to auto-detect virtual function calls from constructors|Enhancement
|SLIDESCPP-2472|Formatting output XML files do not match Slides.Net|Bug
|SLIDESCPP-2473|Automate MathMLTests testing|Task
|CSPORTCPP-3456|An app crashes if an exception is thrown in the lambda-expression of the System::MakeScopeGuard constructor.|Bug
|CSPORTCPP-3429|Porting tests for exceptions|Task
|WORDSCPP-985|Simplify using statements generated by porter|Enhancement
|WORDSCPP-978|Simplify test code generated by porter|Enhancement
|CSPORTCPP-3050|Porting tests: GroupTest, MatchTest, RegexTest, and others|Task
|SLIDESCPP-2484|Porter: Fix porting an unsafe statement block|Task
|WORDSCPP-986|Simplify object initializers generated by porter|Enhancement
|CSPORTCPP-3483|Support CodePorting.Native Cs2Cpp|Task
|SLIDESCPP-2447|Porter: Implement an ability to replace multiple documentation tags with the same name|Enhancement
|SLIDESCPP-2448|Porter: Implement replacing of 'exception' documentation tag|New feature
|SLIDESCPP-2449|Porter: Implement an ability to reduce text for 'see' tag|Enhancement
|CSPORTCPP-3052|Porting tests: MutexTest, SemaphoreTest, ThreadTest, and others|Task
|WORDSCPP-989|Use the auto keyword for variable declarations|Enhancement
|CSPORTCPP-3493|The StopWatch::get_Elapsed returns an incorrect value|Bug
|CSPORTCPP-2064|Put 'override' instructions in ported code|New feature
|CSPORTCPP-3303|Improve CppConstMethodWrapper attribute|Enhancement
|WORDSCPP-961|Implement missing System::Text::UTF7Encoding method|Task
|CSPORTCPP-3051|Porting tests: AutoResetEventTest, EventWaitHandleTest, InterlockedTest and others|Task
|CSPORTCPP-3511|Support CodePorting.Native Cs2Cpp|Task
|WORDSCPP-993|Simplify foreach statements generated by porter|Task
|CSPORTCPP-3053|Porting tests: UTF32EncodingTest, UTF7EncodingTest, UTF8EncodingTest and others|Task
|SLIDESCPP-2497|Add missed EnableRaisingEvents property|Task
|SLIDESCPP-2511|Resolve Aspose.Slides for C++ compilation errors (v20.8)|Task
|CSPORTCPP-3423|Porting tests: StringBuilderTest|Task
|CSPORTCPP-3424|Porting tests: DecimalFormatterTest, NumberFormatterTest|Task
|CSPORTCPP-3425|Porting tests: StreamReaderTest, StreamWriterTest|Task
|CSPORTCPP-3444|Porting tests for collections|Task
|CSPORTCPP-3546|Ensure interfaces have RTTI|Enhancement
|WORDSCPP-955|Improve tiff support|Enhancement
|TASKSCPP-1445|Implement CppUnknownTypeParam/CppValueTypeParam attributes support via configuration file.|Enhancement
|SLIDESCPP-2492|Check asposecpplib for memory leaks (v20.8)|Task
|PDFCPP-1345|Fix Ps tests|Bug
|CSPORTCPP-3522|Memory leak in Xml submodule|Bug
|CSPORTCPP-3556|Support CodePorting.Native Cs2Cpp|Task
|SLIDESCPP-2499|Fix RegressionTests_v20_8.SLIDESNET_35233 test|Bug
|CSPORTCPP-3453|Fixing tests for DateTime|Task
|SLIDESCPP-2392|Fix RegressionTests_v15_11.SLIDESJAVA_34245 test|Bug
|CSPORTCPP-3463|Support CodePorting.Native Cs2Cpp|Task
|WORDSCPP-977|Implement IEnumerable extension methods (Count and First)|New fature
|TASKSCPP-1449|Add XPathDocument ctor from System::Stream|New feature
|SLIDESCPP-2495|Porter: Implement an ability to remove private documentation comments|New feature

## Public API and Backward Incompatible Changes ##

1. Some missing methods of XmlReader and XmlTextReader classes were added.
1. Signatures accepting DateTime and TimeSpan objects were reviewed to handle these by value rather than by const value, as these types now only contain integer ticks counters.
1. Some signatures were fixed for consistency with .Net so that 'override' instructions placed by porter for classes inheriting these would work properly.
1. EnableRaisingEvents property was stubbed in Process class.
1. Missing RTTI members were added into library interface classes.
1. RemoveAliasing() method was implemented in SmartPtr class.
