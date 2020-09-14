---
date: "2020-03-28"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.3"
linktitle: "CodePorting.Native Cs2Cpp 20.3"
menu:
  docs:
    parent: "2020"
    weight: "3"
lastmod: "2020-03-28"
weight: "3"
---

## Major Features ##

1. CodePorting.Native.Cs2Cpp.PortingControl Nuget package was released containing definitions of attributes recognized by the porter.
1. [CppNonConstMethod](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppConstMethod), [CppConstWrapper](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppConstWrapper), and [CppOverride](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppConstMethod) attributes were supported by the porter.
1. VS2019 support was improved and added to the GUI application.
1. Increment and decrement operators are now properly ported for enums.
1. MulticastDelegate constructor was improved to avoid ambiguity in ported code for e. g. "string vs delegate" overloads.

## Minor fixes ##

1. The bug was fixed within SharedPtr class-leading to issues assigning weak pointers to shared ones.
1. XMLNS generation in XmlTextWriter class was made closer to .Net Framework behavior.
1. operator + was fixed for 'String + null object' case.
1. Native's visualizers were improved significantly.
1. db_data_reader.h compilation issues were fixed.
1. GraphicsPath::AddArc() was fixed for some cases.
1. Graphics::DrawString() was improved to use alignment properties.
1. XmlTextReader was fixed to use XmlNamespaceManager property.
1. Some memory management bugs were fixed for XmlTextReader class.
1. EnumValues::GetValueOf() was fixed for the case of integer values.
1. Issue disconnecting MulticastDelegate was fixed.
1. Some pointer implementations were removed from public headers.
1. Signatures of some Array methods were improved by adding 'const' qualifiers to their arguments.
1. Exception-to-object and object-to-exception casts were fixed in ported code.
1. Compilation errors invoking ValueType's methods from ported structs were fixed.
1. Assignment to variables of 'System::Type' type was fixed in ported code.
1. The Const getter deduction algorithm was improved in porter.
1. Missing includes when translating delegates were fixed.
1. Culture/locale fallback mechanism was added.
1. IEnumerator now inherits IDisposable, same as in .Net.
1. Enum::GetUnderlyingType() was supported.
1. System::Collections::List<bool>::ToArray() method was fixed.
1. A crash was fixed when unloading Aspose C++ products dynamically.
1. X509 certificates support was improved. Exception handling was put in line with .Net behavior. Private keys support was extended.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|CSPORTCPP-3170|Support CodePorting.Native Cs2Cpp|Task
|CSPORTCPP-3132|Add Attributes build into a release package|Task
|EMAILCPP-202|Transition to the common version of asposecpplib (CodePorting.Native Cs2Cpp)|Task
|WORDSCPP-930|Improve default xmlns handling|Task
|PDFCPP-1192|fix NullReference in Aspsosecpplib|Bug
|TASKSCPP-1377|Fix invalid shared API reference in db_data_reader.h (asposecpplib)|Bug
|WORDSCPP-855|TestIndexAndTables.TestToc - docx is different|Bug
|SLIDESCPP-2240|Fix GraphicsPath::AddArc calculations|Bug
|SLIDESCPP-2247|Fix the wrong position of text and legends in charts (20.2)|Enhancement
|WORDSCPP-933|XmlTextReader doesn't use XmlParserContext.NamespaceManager property|Bug
|PDFCPP-1198|Fix EnumValuesBase::Parse|Enhancement
|CSPORTCPP-3133|Hide cryptographic provider's implementations|Task
|CSPORTCPP-3188|The ported assignment of the exception object to object fails with a compilation error.|Bug
|SLIDESCPP-2255|Porter: Improve translation of GetHashCode() method invocation for value types|Bug
|WORDSCPP-937|Use the "proxy" approach to extends lifetime of objects|Task
|EMAILCPP-193|Porting Exchange protocol|Task
|WORDSCPP-936|Support for BCP-47 compatible culture names|Task
|PDFCPP-1212|Fix Cs2Cpp porter and Asposecpplib|Task
|CSPORTCPP-2907|Support VS2019|Task
|WORDSCPP-939|Dynamically Loading Words DLL Crashes the App with Access violation Exception|Bug
|WORDSCPP-826|PKCS12/PFX format support|Task

## Public API and Backward Incompatible Changes ##

1. System::Security::Cryptography::ToBase64Transform class was supported.
1. CsToCppPorter::Details::MemoryManagement::ExtendLifetime() method was added. An overload of CsToCppPorter::Details::MemoryManagement::BindLifetime() method was added.
1. System::Threading::Monitor class was supported.
1. WeakReference::operator ## () was supported.
1. System::Collections::Specialized::NameValueCollection class was supported.
1. System::Web::Services::Protocols::SoapHeaderCollection class was made publically available.
1. System::UriBuilder::set_Port() method was added.
1. A new overload of System::Text::RegularExpressions::Regex::Replace() method was added.
