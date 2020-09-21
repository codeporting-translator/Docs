---
date: "2020-05-06"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.4"
linktitle: "CodePorting.Native Cs2Cpp 20.4"
menu:
  docs:
    parent: "2020"
    weight: "6"
lastmod: "2020-05-06"
weight: "6"
---

## Major Features ##

1. .Net Framework 3.5 was supported in CodePorting.Native.Cs2Cpp.PortingControl package for legacy reasons.
1. PFX/PKCS12 support was added which is based on the OpenSSL library.

## Minor fixes ##

1. System::Details::ObjectsBag collection class was added.
1. set_ContentLength() method in System::Net::HttpWebRequest was fixed to make its arguments follow .Net style.
1. Porter now generates proper 'template' keyword when accessing template methods for template arguments.
1. 'min' and 'max' methods were properly escaped everywhere to avoid compilation errors on GCC and Clang.
1. Extra references were removed from CodePorting.Native.Cs2Cpp.PortingControl package. It no longer depends on any class that is not present in .Net Framework 2.0.
1. A bug was fixed for System::SmartPtr class causing incorrect behavior for assigning two shared pointers which point to the same object but manage lifetimes of the different ones.
1. Exceptions are thrown by a constructor of System::Guid class was put in line with .Net behavior.
1. When porting generic classes, porter now generates friend declarations to other specializations of same class to put access rights in line with such in C#.
1. DPI values are now correctly saved to Tiff format.
1. Different Tiff compression types were supported.
1. Tiff pixel format was supported.
1. XmlNode class now inherits IXPathNavigable and can be used with XslCompiledTransform, the same as in .Net.
1. Botan license file was actualized.
1. Several previously unsupported methods of System::Collections::Generic::LinkedList class were implemented.
1. System::Diagnostics::Trace::Flush() method was implemented.
1. StaticCast conversion was fixed for 'String to String' case as it gets generated for some constructs.
1. Several missing methods of XmlTextWriter and XmlWriter classes were implemented.
1. Include string being generated for System::DayOfWeek enum was fixed.
1. user2.cmake class is now included at the end of the ported project's CMakeLists.txt.
1. String comparison was fixed for some cases in 'ordinal, ignore case' mode.
1. Some previously unimplemented methods of WaitHandle class were supported.
1. 'foreach' statement porting was fixed for IEnumerable<T>.
1. System::Nullable::GetValueOrDefault() method was implemented.
1. [CppForceSharedApi](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppForceSharedApi) attribute is now recognized by porter when applied to static constructors.
1. Behavior of System::Collections::Generic::Stack::Stack(int) constructor was fixed. Implementation of System::Collections::Generic::Stack::ToArray() was improved.
1. Assert.AreEqual() is now working correctly in ported code when its arguments are of different boxed types.
1. More LINQ methods were supported at IEnumerable level.
1. TestCaseSource attribute is now ported properly for multiargument tests with a single method.
1. Attributes are now properly retrieved from base classes through TypeInfo class. TypeInfo::GetCustomAttributes(attribute type, inherit) was fixed.
1. TypeInfo::IsSubclassOf() and TypeInfo::IsAssignableFrom() methods were supported.
1. Several scenarios of using SOAP features were fixed.
1. DateTime::Parse() was fixed for the case of milliseconds being present in the date string.
1. System::Collections::Generic::SortedList::get_Capacity() method was fixed.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|WORDSCPP-935|Increase the performance of DocumentBase.AddHangingNode and RemoveHangingNode methods|Task
|WORDSCPP-714|Implement SystemPal.ExecuteWebRequest() and related methods|Bug
|CSPORTCPP-3198|Investigate the state of 'detect_const_methods' option|Task
|CSPORTCPP-3211|Add .Net Framework 3.5 build for CodePorting.Native.Cs2Cpp.PortingControl|Task
|PDFCPP-1223|template keyword for gcc compiler|Task
|CSPORTCPP-3253|Add new license pdf file with OpenSSL and skcms information|Task
|CSPORTCPP-3238|Fix SmartPtr assignment operator issue with aliasing constructor|Bug
|SLIDESCPP-2304|Porter: Fix nunit_categories option priority|Task
|TASKSCPP-1382|Fix GUID creation from a string in the case when the string is invalid or empty.|Task
|TASKSCPP-1387|Generate self-friendship for parametrized (generic) classes to provide .Net members accessibility behavior.|Task
|TASKSCPP-1388|Improve Math.Sign compatibility to .Net|Task
|SLIDESCPP-1714|Implement DPI handling for Tiff format|Task
|SLIDESCPP-1715|Implement Tiff compression types support|Task
|SLIDESCPP-1716|Implement Tiff pixel format support|Task
|PDFCPP-1239|Fix XslCompiledTransform|Bug
|CSPORTCPP-3258|Support CodePorting.Native Cs2Cpp|Task
|EMAILCPP-206|Preparing Aspose.Email for C++ release 20.1|Task
|SLIDESCPP-2310|Incorrect 'foreach' statement translation for IEnumerable<T> type|Task
|TASKSCPP-1390|Implement Nullable<T>.GetValueOrDefault() method (parameter-less)|Task
|WORDSCPP-946|Rework Aspose.Common string related classes|Task
|SLIDESCPP-2329|Fix InvalidCastException errors when running FunctionTests and FormulaTests (v20.4)|Bug
|PDFCPP-1236|porting PUB|Task

## Public API and Backward Incompatible Changes ##

1. MethodArgumentTuple class was fixed to work properly with const methods and const parameters.
1. '[nunit_categories](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-nodes/#Hnunit_categories)' option's behavior was changed. From now on, 'include' clauses have higher priority than such of 'exclude'.
1. System::Math::Sign() now returns int in all cases, same as in .Net.
1. Several SOAP-related classes were supported in System::Web namespace.
1. System::Collections::IDictioary::TryGetValue() method is now const. Poter is fixed to port is as const by default.
