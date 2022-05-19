---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Cmake Support"
linktitle: "Cmake Support"
menu:
  docs:
    parent: "Developer Guide"
    weight: "8"
lastmod: "2019-05-28"
weight: "8"
---

Translated projects are being built by means of Cmake. Specify target platform you want to build your binaries for. Use standard Cmake means to do so. If building for platform different from the one you were using to translate your binaries (e. g. for Linux), copy all files of the translateded project to this new platform and make sure to supply correct references to location of CodePorting.Translator Cs2Cpp distribution package on this new platform.

However, when doing so, make sure that you have a pre-built codeporting.translator.cs2cpp.framework dynamic library for this platform. Only a fixed set of platforms is supported. For example, to build translated code using Microsoft Visual Studio 2017 or 2019 using 64-bit compiler in debug mode, you need codeporting.translator.cs2cpp.framework_vc14x64d.dll pre-built library. See documentation for your release to find out which platforms are supported by the version you are using.

When running Cmake, one can normally specify some options. Please note that some available options are listed in bin\translator\cmake\BuildSettings.cmake. However, never change these options if linking your code with default codeporting.translator.cs2cpp.framework dynamic library as the set of options must match between the library and the translated project. User doesn't have any control over the options used to build libraries; however, some special assemblies may be provided with some (mostly debugging-related) Cmake options changed. If using such special version of the library, refer to instructions related to it.

Version compatibility checker feature ensures that the set of enabled options is same between pre-built system libraries and all of the translated modules. See **version_compatibility_check_mode** option description in [Configuration File Options](/translator/cs2cpp/developer-guide/codeporting-translator-cs2cpp-configuration-file/configuration-file-options/) section for more details.