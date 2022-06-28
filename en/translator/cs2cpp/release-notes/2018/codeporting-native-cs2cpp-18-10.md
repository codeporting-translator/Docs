---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 18.10"
linktitle: "CodePorting.Native Cs2Cpp 18.10"
menu:
  docs:
    parent: "2018"
    weight: "3"    
lastmod: "2019-05-28"
categories:
- "release_notes_2018"
weight: "3"
---

## Major Features ##

Following improvements and fixes are applied in this regular monthly release:

1. Null reference error fixed when accessing namespace of XmlAttribute without any namespace set
1. Fixed runtime error when setting InnerText for XmlNode with no parent XmlDocument
1. Added missing overloads of XslCompiledTransform::Tranform() methods
1. Behavior of XmlTextWriter class now matches such of .Net class regarding spaces and EOL symbols
1. Fixed runtime error when assigning A object only referenced by B object to SmartPtr holding last reference to B
1. Several potential memory leaks related to WeakPtr usage are fixed
1. Support of several System::ComponentModel classes was added. Related API is fixed
1. Issue with XmlTextReader::ReadString() invalidating reader if element is empty is fixed
1. Incorrect work of Convert::ToString() function with DateTime type is fixed
1. Translation of concatenation between string and enum types is fixed
1. Detection of String vs bool overload is switched to fully automatic mode. CppForceStringParam attribute is still available e. g. for String vs TypeInfo cases
1. Translation of surrogate pairs inside string literals is fixed
1. Errors detected during Xml files parsing no longer go into stderr by default. To change this behavior, use ASPOSE_LIBXML2_ERROR_OUTPUT_CHANNEL system variable with possible values of 'stderr', 'stdout', 'debug', 'null' or 'file:<filename>'
1. Platform checks are improved in Nuget package
1. Fixed porter not taking config into account when translating System.Char references
1. 64-bit integer to string conversions behavior was synchronized with .Net when it comes to trailing and leading spaces. Also, performance is improved
1. Shorter, more convinient directory name is provided inside release package

## Full List of Issues Covering all Changes in this Release ##


| Key | Summary | Category
---| ---|  ---|
|CSPORTCPP-2242|Improve Nuget platform checks|Improvement
|CSPORTCPP-2164|Remove char16_t references from porter|Improvement
|CSPORTCPP-2264|Improve directory naming in release archive|Improvement
|WORDSCPP-654|libxml2 prints warnings and errors to stderr|Improvement
|WORDSCPP-621|xmlns attribute returns NullPtr in C++|Bug
|WORDSCPP-625|XmlNode.SetInnerText cause NullReferenceException|Bug
|WORDSCPP-609|Manually implement or port XmlComparer|Task
|WORDSCPP-615|Xml written by ported C++ has minor differences from Xml writen by .NET|Bug
|CSPORTCPP-2182|Fix SmartPtr issue|Bug
|WORDSCPP-648|XmlTextReader.ReadString() method should Call MoveToElement() internally.|Bug
|PDFCPP-767|Fix CsToCppPorter - conversion to string|Bug
|WORDSCPP-532|Incorrect overload selection for string literals and nullptr vs bool|Bug
|WORDSCPP-663|Incorrect porting of string literal with surrogate pairs|Bug
|WORDSCPP-655|System::Convert::ToInt32(string) can't parse string with spaces|Bug

## Public API and Backward Incompatible Changes ##

1. Several previously unsupported overloads of System::Xml::Xsl::XslCompiledTransform::Transform() method are added
1. Support of the following classes is added: System::ComponentModel::AsyncCompletedEventArgs, System::ComponentClass::BackgroundWorker, System::ComponentWorker::ProgressChangedEventArgs, System::ComponentModel::RunWorkerCompletedEventArgs, System::Reflection::TargetInvocationException
1. Support of System::Threading::WaitHandle::WaitOne(const System::TimeSpan &timeout) method is added
1. char16_t_array type renamed to system_char_array. const_char16_t_array type renamed to const_system_char_array
1. System::Char::IsAsciiWhiteSpace() method is added
