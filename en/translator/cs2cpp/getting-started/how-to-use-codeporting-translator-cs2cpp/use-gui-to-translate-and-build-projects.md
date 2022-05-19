---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "How to Use GUI to translate and build Projects"
linktitle: "How to Use GUI to translate and build Projects"
menu:
  docs:
    parent: "How to use CodePorting.Translator Cs2Cpp"
    weight: "2"
lastmod: "2019-06-27"
weight: "2"
---

## Overview ##

“CodePorting.Translator Cs2Cpp GUI” application provides Graphical User Interface (GUI) to a command-line application CodePorting.Translator Cs2Cpp. The “CodePorting.Translator Cs2Cpp GUI” application’s executable CodePorting.Translator.Cs2Cpp is located in bin/gui subdirectory of “CodePorting.Translator Cs2Cpp” installation directory.

"CodePorting.Translator Cs2Cpp GUI" provides an easy and convenient way to perform typical operations when translating C# projects to C++ and building them afterwards (by using Cmake and corresponding compiler). Though one can use command-line application "CodePorting.Translator Cs2Cpp" directly, it is more suitable for automation (e.g. for scripting in continuous integration environments), while window-based "CodePorting.Translator Cs2Cpp GUI" provides more flexibility and control over the translating process when performing translating manually.

Here is a screenshot of the CodePorting.Translator Cs2Cpp GUI application’s main – and the only – window:

![](../csPorter_GUI.PNG)

The Main Menu bar located in the top row of the application’s widow provides access to all functions supported by the application – translating, building, saving/loading a workspace etc.

Below the Main Menu are located buttons and combo boxes that provide access to and control over the most essential functionality of the application – translating, generating project or makefile from translated source code, and building the translated C++ project. The buttons in this area serve as shortcuts to the corresponding subitems of the Action menu item of the Main Menu.

Next goes the middle area of the window, which is divided into two logical subareas – left and right. The left area is associated with the C# project to be translated. It contains controls that allow one to specify path to the translator configuration file and path to the C# project to be translated. Also it contains a tree view control that displays the content and structure of the specified C# project. The right subarea of the window’s middle area is associated with the output directory, where translator will put files generated during poring. This subarea contains an input box that allows one to specify path to the output directory and a tree view control that displays the contents of the specified directory.

The bottom area of the window contains a text box labelled "Output" to which the log of the activity in the application is printed including the output of any external applications executed by "CodePorting.Translator Cs2Cpp GUI" (e.g. “CodePorting.Translator Cs2Cpp” or Cmake).

## Translating a C# project ##

Before a C# project can be translated, some information needs to be passed to "CodePorting.Translator Cs2Cpp GUI" application: path to the .csproj file of the C# project to translate, path to the output directory where translator will put all generated files including C++ source files and optionally path to translator configuration file containing translator configuration options and settings can be specified.

### Specifying a C# project to translate ###

Path to the .csproj file of the C# project to be translated is obligatory and must be specified in the text box containing corresponding prompt: “Enter path to C# project to translate”. The path can be specified by typing it right into the text box or by choosing it in a filesystem tree window that pops-up when button labeled “…” located beside the input box is clicked. When the path to the .csproj file is specified and if it is correct, the content and the structure of the specified C# project is displayed in a tree-view control below the corresponding input box.

### Specifying a path to the configuration file ###

Path to the translator configuration file is optional. It can be specified in the input box containing the following prompt: “Enter path to configuration file”. The path can be typed into the input box or selected from a file system tree window that opens when button labeled “...” located to the right from the input box is clicked. In order to instruct the "Codeporting to C++ GUI" application to use default configuration, checkbox labeled “use default” located to the right from the input box has to be set, in which case the input box becomes disabled and the value specified there - if any - is ignored by the application.

### Specifying the output directory ###

The last thing translator needs to know to perform translating is the output directory to put the generated files - result of poring - into. The path to the directory is obligatory and must be specified in the input box located on the right half of the window and containing the prompt “Enter path to the output directory”. The path can be typed into the input box or chosen in a filesystem tree-view window that is opened by clicking on a button labeled “…” to the right from the input box. When the path to the output directory is specified and if it is correct, the content of the specified directory is displayed in the tree-view control below the corresponding input box.

Note that if the specified path to the output directory is correct and the corresponding directory contains file named CMakeLists.txt, "CodePorting.Translator Cs2Cpp GUI" assumes that this directory contains C++ and Cmake configuration files generated by translator and buttons labeled “Generate project/makefile” and “Build” and their corresponding items of Actions submenu item of Main Menu become enabled. The application does not care if the files located in the output directory were generated during current session or anytime in the past or maybe even not by "CodePorting.Translator Cs2Cpp GUI". If the application finds some Cmake configuration files in the output directory, it enables functionality to work with them by enabling corresponding buttons and menu items.

### Translating ###

When all necessary parameters are specified, translating can be performed. Translating action is initiated by clicking button named “Translate” or a Main Menu item Action->”Translate project”. If one or more necessary parameters are not specified, the translating is not initiated and the problem input boxes are marked with red border, suggesting that they must be filled.

When translating is running, all other actions are not allowed to be initiated, therefore the corresponding buttons and menu items become disabled until translating finishes.

While translating action is running, the log is written to the text box labelled “Output”. When translating finishes, the log and result of the action can be seen in the “Output” textbox.

Depending on the size and complexity of the C# project being translated, the running time of the poring action may vary in quite wide range – from just a couple of seconds to tens of seconds and minutes. Main Menu item Action->Cancel will abort the running action.

When translating finishes successfully, the output directory contains the results of translating - C++ source files and Cmake configuration files generated by translator.

## Configuring and building C++ project ##

Besides translating a C# project, "CodePorting.Translator Cs2Cpp GUI" provides means to configure and build the resulting C++ project using Cmake. This functionality requires Cmake to be installed in the system and path to cmake.exe to be present in PATH environment variable.

### Specifying the build system and configuration ###

Two combo boxes to the right from button labeled “Translate” allow to specify a build system to build C++ project with and configuration of the C++ project to build. The specified build system and configuration are both used by “Generate project/makefile” and “Build” actions. And both these values are ignored during translating.

Though "CodePorting.Translator Cs2Cpp GUI" generates C++ source code and Cmake configuration files that allow targeting Linux build systems, "CodePorting.Translator Cs2Cpp GUI" does not provide means to do that for one reason - "CodePorting.Translator Cs2Cpp GUI" is a Windows application while configuration and building for Linux has to be done on Linux system. Thus the build system combo box contains only those build systems, which are available on Windows – Visual Studio

### Generating project/makefile ###

Depending on the specified build system the translated C++ project is configured for, Cmake will generate either a Visual Studio project file (when configuring for Visual Studio build system). The button labeled “Generate project/makefile” initiates configuration process by invoking Cmake with the proper parameters. The output produced by Cmake and the result of the operation is printed to and can be seen in the Output text box.

### Building translated C++ project ###

After the translated C++ project is configured for particular build system, it can be built with that build system. Button labeled “Build” initiates the corresponding action. Building is perform using Cmake and assumes that all necessary components of the specified build system are installed. The output produced by Cmake and the result of the operation is printed to the Output text box.

### Translating C# project and configuring and building C++ project with a single click ###

Main Menu item "Action > Whole workspace sequence" is equivalent to clicking buttons “Translate”, “Generate project/makefile” and “Bulid” one by one.

## Cancelling actions ##

Main Menu item "Action > Cancel" aborts the running action.

When any action is in progress (including workspace loading or saving, project translating, configuring or building, etc.), you can't run other actions, change workspace parameters or close the program window.

## Workspace management ##

Workspace is a set of parameters specified by the user in the "CodePorting.Translator Cs2Cpp GUI" application's window, which includes:

* Path to the .csproj file that represents the C# project to translate
* Path to the Translator configuration file
* The state of  the “use default” checkbox
* Path to the output directory
* Target C++ build system
* Target C++ project configuration

Once specified these values can be conveniently stored to a file and then later loaded back.

### Creating a workspace ###

The new workspace is created implicitly everytime "CodePorting.Translator Cs2Cpp GUI" application is started. There is no need (and no way) to create it manually.

### Saving workspace ###

Main Menu item "File > Save workspace as..." opens a window in which name and location of the file to which the workspace should be saved can be specified. When a workspace is saved to a particular file, it is associated with that file. Main Menu item "File > Save workspace" saves current workspace to the file it is associated with, overwriting the file. If the current workspace is not associated with any file yet, “File > Save workspace” Main Menu item behaves just like “File > Save workspace as…”.

### Opening existing workspace ###

Main Menu item "File > Open workspace..." opens a window where one can choose a workspace file. Once a correct file is chosen, all values stored in it are loaded and corresponding controls on the main window are filled with the loaded values.

## Getting help ##

Use "Help > Online help" menu to get to online help page.

Use "Help > About..." menu to get basic program and version information.

## Tools location ##

"csPorer for C++ GUI" always uses Translator and Library located in the same installation pack. "CodePorting.Translator Cs2Cpp GUI" cannot be used with different Translator or Library version.

To configure and build translated C++ projects, Cmake must be installed and path to cmake.exe must be added to the PATH environment variable. To check if this is the case, run Command line, enter "cmake /?" command and press Enter. If Cmake is installed propertly, you should be geting Cmake help output.

Also, to build translated C++ projects, build systems must be installed on the system. If you're building projects from "CodePorting.Translator Cs2Cpp GUI", Cmake is responsible for detecting build systems for you.

If you're configuring and/or building your translated C++ project manually, use ASPOSE_ROOT Cmake variable to specify path to installation of "CodePorting.Translator Cs2Cpp" you're using. Make sure the installation is the same you use Translator from. Typical Cmake command line should look like below:

```
cmake -G "Visual Studio 15 2017" -DASPOSE_ROOT#"path\to\csporter\installation" path\to\translated\project
```
