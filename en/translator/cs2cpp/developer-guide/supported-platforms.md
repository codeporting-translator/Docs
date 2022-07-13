---
date: "2022-07-13"
author:
  display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "Supported platforms"
linktitle: "Supported platforms"
menu:
  docs:
    parent: "Developer Guide"
    weight: "12"
lastmod: "2022-07-13"
weight: "12"
---

There are different requirements for running CodePorting.Translator.Cs2Cpp (including the GUI) and for building/running the translated code.

## Running the Translator ##

These are the system requirements to run CodePorting.Translator.Cs2Cpp either as the command line tool or via GUI:

1. Windows 7 or above or Windows Server 2012 R2 or above.
1. .Net Framework 4.7.2 or above.
1. MSBuild from Visual Studio 2017 or above (any edition like Build Tools, Community or Professional will do).

It is currently not possible to run the translator on non-Windows platforms. However, the code which was generated on Windows can then be compiled for Windows, MacOS or Linux.

## Building the translated code ##

After the code is translated, it can be built for any supported platform. The codeporting.translator.cs2cpp.framework library is provided for each of these.

### Windows ###

These are the requirements for building the translated code:

1. Windows 7 or above or Windows Server 2012 R2 or above.
1. Visual Studio 2017 or above.
1. CMake 3.16 or above.

The translated code can be built for both 32-bit and 64-bit platforms.

### Linux ###

These are the requirements for building the translated code:

1. The Linux distribution for x86_64 platform (aka x64, aka AMD64, aka Intel 64), e. g. Ubuntu Linux or Debian Linux.
1. GCC 6 or above of Clang 3.9 or above.
1. libstdc++ 6.0.28 or above.

Only 64-bit platform is supported.

### MacOS ###

These are the requirements for building the translated code:

1. MacOS 12.1 or above.
1. Apple Clang 13.0 or above.

Only 64-bit platform is supported.

## Running the translated code ##

The translated code you built can run on any of the supported platforms.

### Windows ###

These are the requirements to run the translated code:

1. Windows 7 or above or Windows Server 2012 R2 or above.
1. Microsoft Visual C++ Redistributable package for the Visual Studio you used to build the translated code, but no lower than 2017.

Both 32-bit and 64-bit modes are supported.

### Linux ###

These are the requirements to run the translated code:

1. The Linux distribution for x86_64 platform (aka x64, aka AMD64, aka Intel 64), e. g. Ubuntu Linux or Debian Linux.
1. libstdc++ you linked against or above, but not lower than 6.0.28.

Only 64-bit platform is supported.

### MacOS ###

These are the requirements to run the translated code:

1. MacOS 12.1 or above, capable of running the code built for Intel architecture (either directly or via Rosetta).

Only 64-bit platform is supported.