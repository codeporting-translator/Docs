---
date: "2021-05-19"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "Qt support"
linktitle: "Qt support"
menu:
  docs:
    parent: "Developer Guide"
    weight: "3"
lastmod: "2021-05-19"
weight: "3"
---

[Qt](https://www.qt.io/) is a widely used C++ framework. It provides the libraries for many usecases. There is also an IDE available which is called Qt Creator. This page contains information on how these can be used with CodePorting.Translator Cs2Cpp.

## Qt data types conversion ##

Qt libraries provide many general use data types (e. g. strings, collections, I/O primitives and so on). They also contain many graphical types (colors, brushes, pens and so on).

CodePorting.Translator Cs2Cpp also provides types for such applications. However, these types follow .Net API in order for the translated code to work properly. Qt is not used by CodePorting.Translator, and it is not required to use Qt in order to use CodePorting.Translator Cs2Cpp. On the other hand, CodePorting.Translator Cs2Cpp types are not compatible with their Qt analogs.

Those customers who use both CodePorting.Translator and Qt, may use helper functions available [here](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/qt_helpers). Please click on below links for more information:

* [Helpers for general use data types](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/blob/master/qt_helpers/include/qtcorehelpers.h);
* [Helpers for graphical data types](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/blob/master/qt_helpers/include/qtguihelpers.h);
* [Examples of conversion between general use CodePorting.Translator Cs2Cpp data types and Qt ones](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/blob/master/qt_helpers/tests/test_qtcorehelpers.cpp);
* [Examples of conversion between graphical CodePorting.Translator Cs2Cpp data types and Qt ones](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/blob/master/qt_helpers/tests/test_qtguihelpers.cpp).

There are two major concepts present here: converting and wrapping.

* Converting means taking an existing value and creating a value of a different type which is completely independent from it, but contains full equivalent of the data. This works for lightweight values like strings, colors, dates and so on.
* Wrapping means creating an object which behaves as an instance of a target type, but actually proxies the calls into source type object it was created for. This works for heavy entities like buffers and files where copying would cause a performance penalty or alter the logic.

The below code demonstrates some typical usage of helper functions.

{{< highlight cpp >}}
System::String string1 = u"abc"; // CodePorting.Translator Cs2Cpp string
QString string2 = Aspose::QtHelpers::Convert(string1); // Qt string
System::String string3 = Aspose::QtHelpers::Convert(string2); // Back to CodePorting.Translator Cs2Cpp string

QFile file("QtCoreHelpers.WrapQFile.txt"); // Qt file
System::SharedPtr<System::IO::Stream> stream = Aspose::QtHelpers::Wrap(file); // Now this stream can be used to read from Qt file in translated code.
{{< /highlight >}}

## Qt Creator debugging helpers ##

When working with CodePorting.Translator Cs2Cpp types in Qt Creator environment, it is possible to use debugging helpers to inspect the values of the variables. [asposetypes.py file](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/blob/master/qtcreator_debugging_helpers/asposetypes.py) contains such helpers. Please consult [the readme file](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/blob/master/qtcreator_debugging_helpers/README.md) for the information on how to install and use these. For more information, including the sample project, please refer to [this directory in our GitHub](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/qtcreator_debugging_helpers).