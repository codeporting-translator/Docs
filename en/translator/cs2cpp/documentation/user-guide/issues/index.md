---
order: "8"
navTitle: "Issues"
---

# Issues Reference #

When the translator encounters a problem, it outputs a special message to the console called an "issue." The format of such a message is as follows:

```txt
[Severity ID] Message 'token(s)'.
  location
```

Where:

**Severity**: the "danger" level of the situation from the translator's perspective. It can be of the following types:

* SILENT - the message does not appear in the log, is not added to journal, and is not included in the error count. This is necessary when setting up custom severity rules if you want to completely suppress a specific message or group of messages (using wildcards).
* INFO - the least dangerous situation — simply a notification to the user about a non-trivial situation that was successfully resolved by the translator and does not require mandatory user intervention.
* WARNING - a potentially hazardous situation that will not necessarily lead to errors in the resulting code but, should they occur, will indicate to the user where to look for the problem. Unless the `-w2` command-line flag is set, warnings do not affect the execution result of the console application. They are displayed in yellow in the console.
* ERROR - a notification regarding a situation that is virtually guaranteed to result in the generation of erroneous or non-compilable C++ code. Except when the `-w0` flag is used, this always causes the program to terminate with exit code 1 (error). It requires the user to modify the target C# code or configuration files, or to use additional command-line flags. These messages are displayed in red in the console.
* FATAL ERROR - A fatal error that renders the continuation of the translation pointless and immediately terminates the application with error code. It is also displayed in red.

The strictness level can be configured via "severity rules," which can be specified on the command line using the -sr flag or in [configuration file](../configuration-file/nodes.md#severity_rule). For example, `-sr T70 Fatal` will result in the translation process terminating immediately upon encountering unsupported code, without waiting for all files to be processed, and `-sr G* Info` will convert all C++ file generation issues into ordinary informational messages.

> Keep in mind that lowering the severity of certain messages does not guarantee that the translator will resolve the situation on its own and generate correct code; it merely affects the message category in the log and the resulting execution code of the translator application.

**ID** - the unique message identifier used to locate the message in the documentation or manage its severity level. It consists of two parts: the message type and a unique number (typically two digits). Message types indicate the area where the message originated:

* CLI - a message regarding [command-line issues](cli.md), for example, invalid or obsolete switches or their parameters.
* C - [common issues](common.md) related to the translator environment, loading the project to be translated and the configuration file, access to files, etc.
* T - [issues with the translation](translation.md) of C# files. In the vast majority of cases, they indicate code that, for one reason or another, cannot be correctly translated into C++.
* G - the [problems with generating](generation.md) the resulting C++ files are currently largely related to the generation of include directives.

**Message** - a text description of the problem that briefly conveys its essence.

**Tokens(s)** - additional information that fits into the messages and makes them more specific, facilitating the search for the cause of the problem.

**Location** - information about the location (file, syntactic node) where this problem occurred.

In addition to the console window, all errors are also recorded in the translation log (located in the output directory alongside other logs in the file *translator.log*).
