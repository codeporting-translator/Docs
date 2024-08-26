# CodePorting.Translator Cs2Java translator: console application

One of the applications that provide access to the functions of the CodePorting.Translator Cs2Java product is designed as a console application.

Run the CodePorting.Applications.Console.exe file, passing it the necessary parameters via command lines.

The choice of a console application is determined by the characteristics of the task and user preferences. The functionality of all translator applications implemented within the CodePorting.Translator Cs2Java product are the same.

A **console** application is a program that requires no graphical user interface to run and can be completely run from a **command line** environment.

The **command line**, also called the Windows command line, command screen, or text interface, is a user interface that's navigated by typing commands at prompts, instead of using a mouse.

Policies that are set to store global configuration settings can lead to unexpected behavior in the console application. For example, if you don't explicitly specify the target directory for code translation results in the command line arguments (the`-d`parameter), the value of this parameter can be taken from the global configuration and, then, the translation results can end up in an unexpected place. For example, if someone ran a GUI application before and specified the path to place the results in its configuration dialog box, that path will be used to place the results of the console application as well. You should keep such configuration peculiarities in mind and, if necessary, explicitly prescribe the values of all parameters.

Summary of CONSOLE statement functions



|CONSOLE Statement Keyword|Description|See The Topic|
| - | - | - |
||**Available parameters:**||
|'-help', '-h'|||
|'-conversionworkflow', '-wf'|||
|'-sources', '-s'|||
|'-destination', '-d'|||
|'-loggerconfiguration', '-lc'|||
|'-logginglevel', '-l'|||
|'-maxthreads', '-t'|||
|'-quiet', '-q'|||
|'-configuration', '-c'|||
|'-workertask', '-wt'|||
|'-about', '-a'|||
|'-loadedassemblylist', '-la'|||
||**Available extension parameters:**||
|'-compareandsync', '-compareandsync(compare andsync)'|||
|'-oldfolderpath', '-oldfolderpath(comparea ndsync)'|||
|'-newfolderpath', '-newfolderpath(comparea ndsync)'|||
|'-outputfolderpath', '-outputfolderpath(compa reandsync)'|||
|'-migrateconfig', '-migrateconfig(migratec onfig)'|||
|'-configsourcepath', '-configsourcepath(migra teconfig)'|||
|'-destinationpath', '-destinationpath(migrat econfig)'|||
||**Extension assembly name:**||
|'-analyze', '-analyze(analyze)'|||
|'-sourcepaths', '-sourcepaths(analyze)'|||
|'-destinationpath(analyz e)'|||
||**Available parameter prefixes:**||
|'-', '/', '\'|||

Working with code fragments (snippets)

Place the code snippet into some file with cs extension. For example, let's take the "C#-001-1 code snippet" in question and put it into the file test.cs. Let's translate the code snippet with the following command.
```
CodePorter.Applications.Console.exe -wf {634C1942-3E89-4312-9891-8A0671B4D7F1} -s <path-to-sourse>\test.cs -d <path-to-destination-directory>
```
**Make sure** to execute it from the directory where CodePorter.Applications.Console.exe is located, otherwise license file will not be detected.
The result of the translation will be the test.java file. The content of the file must match "Java-001-1 code snippet".

Note that <path-to-sourse> must be present. It doesn't matter if this path is absolute or relative (even in the form .\ ), but the path must be there, otherwise porting will fail.

Working with a Visual Studio IDE project or solution

Working with projects or solutions from the Visual Studio IDE, is almost completely identical to working with a snippet. The only difference is that you do not need to specify a snippet placement file, but a project or solution file.

for the project
```
CodePorter.Applications.Console.exe -wf "{634C1942-3E89-4312-9891-8A0671B4D7F1}" -s <path-to-sourse>\<name-of-project>.csproj -d <path-to-destination-directory>
```
Or, to solution
```
CodePorter.Applications.Console.exe -wf "{634C1942-3E89-4312-9891-8A0671B4D7F1}" -s <path-to-sourse>\<name-of-solution>.sln -d <path-to-destination-directory>
```
Available workflows (-wf argument values) :
| Workflow name | GUID |
| -- | -- |
| Translate C# code to Java | {634C1942-3E89-4312-9891-8A0671B4D7F1} |
| Simplify C# code using ILSpy | {EEE636EA-A492-4909-A0D6-BA83FCB5C866} |
| Simplify C# code using ILSpy and translate to Java | {F321F095-2C33-4B29-9B8E-9B0783F612B0} |