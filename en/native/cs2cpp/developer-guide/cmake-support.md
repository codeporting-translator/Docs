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
## Cmake Support ##

Ported projects are being built by means of Cmake. Specify target platform you want to build your binaries for. Use standard Cmake means to do so. If building for platform different from the one you were using to port your binaries (e. g. for Linux), copy all files of ported project to this new platform and make sure to supply correct references to location ofCodePorting.Native Cs2Cpp distribution package on this new platform.

However, when doing so, make sure that you have a pre-built aspose_cpp_* dynamic library for this platform. Only a fixed set of platforms is supported. For example, to build ported code using Microsoft Visual Studio 2015 using 32-bit compiler in debug mode, you need aspose_cpp_vc140d.dll pre-built library. See documentation for your release to find out which platforms are supported by the version you are using.

When running Cmake, one can normally specify some options. Please note that some available options are listed in ```bin\porter\cmake\BuildSettings.cmake```. However, never change these options if linking your code with default aspose_cpp dynamic library as the set of options must match between the library and the ported project. User doesn't have any control over the options used to build libraries; however, some special assemblies may be provided with some (mostly debugging-related) Cmake options enabled. If using such special version of the library, refer to instructions related to it.

Version compatibility checker feature ensures that the set of enabled options is same between pre-built system libraries and all of the ported modules. See **version_compatibility_check_mode** option description in [Configuration File Options](/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/) section for more details.
