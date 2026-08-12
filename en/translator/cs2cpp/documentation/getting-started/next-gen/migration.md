---
order: "0"
navTitle: "Migration"
---

# Migration from old translator #

The new translator is being developed to ensure the most comfortable and seamless migration from the old translator and the new translator's console application (**"bin\code_translator\CodeTranslator.Cs2Cpp.Console.exe"**), accepts input familiar to the original translator.

As a rule, the launch of the old translator is often embedded in various build scripts written by teams for automating the release process, and replacing the call of the old translator with a new one can require a lot of routine work. Moreover, in case of failure of the transition, the task arises to roll back from the old one for a while, so as not to freeze the process of regular releases publishing. To solve this problem, we made a modification to the old translator, which (except of PUBLIC_RELEASE version) allows you to launch the new translator from within the old one without any edits to the calling scripts. To do this, simply update the original translator to the latest version, and then place a text file named "**Roslyn.Translator.Locator.txt**" in the translator's working directory (by default, this is "asposecpplib\bin\translator\", but build scripts can set their own directories). The file must contain the full direct path to the executable file of the console Cs2Cpp application of the new translator. For example:

> C:\aspose\csporter\cpp\asposecpplib\bin\code_translator\CodeTranslator.Cs2Cpp.Console.exe

After this, each run of the original translator will result in the new translator being started as a child process.

As [mentioned earlier](index.md), the new translator fully understands the configuration files and attributes of the original one, so no code preprocessing is required. However, there are some features that should be taken into account:

1. The new translator works only with semantically correct projects. In other words, the projects to be translated must compile correctly on the given machine. This means, in particular, installing the correct version of the .NET SDK and other components that affect this. Otherwise Roslyn will not be able to obtain the correct semantics, and as a result, we will get a translation error.
1. Although the new translator supports [features of new versions of the C# language](new-features.md), this does not mean that it supports them all. Roslyn itself may well correctly parse some new construction, but at the same time translator may "quietly" ignore unfamiliar syntax elements, and then, without issuing any errors, produce incorrect C++ code. The language is constantly evolving, new classes and methods are added to Roslyn, and there is no way to determine whether they are all processed or not. Therefore, control over such errors is still entirely up to the user.
1. Despite the fact that the new translator passes almost all tests of the old translator, this does not mean that it correctly handles 100% of all cases. Any new project contains unique code, and it is simply impossible to cover it with tests entirely. Therefore, when switching to a new translator, it is absolutely normal to get a couple of translation errors, and then a couple dozen more errors in the translated code. As a rule, they are easily fixed within a few days. At the moment, the elimination of such errors lies with the translator developers, however, as practice has shown, members of product teams can easily fix them themselves.

The new translator is currently under development and is essentially a "sandbox" where teams can add the functionality they need or fix bugs. Any team can create their own workflow, where they can add any necessary manipulations or phases of code processing, analysis and generation of the necessary metainformation.
