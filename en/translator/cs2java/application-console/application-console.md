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
CodePorter.Applications.Console.exe -m p -s <path-to-sourse>\test.cs -d <path-to-destination-directory>
```
The result of the translation will be the test.java file. The content of the file must match "Java-001-1 code snippet".

Note that <path-to-sourse> must be present. It doesn't matter if this path is absolute or relative (even in the form .\ ), but the path must be there, otherwise porting will fail.

Working with a Visual Studio IDE project or solution

Working with projects or solutions from the Visual Studio IDE, is almost completely identical to working with a snippet. The only difference is that you do not need to specify a snippet placement file, but a project or solution file.

for the project
```
CodePorter.Applications.Console.exe -m p -s <path-to-sourse>\<name-of-project>.csproj -d <path-to-destination-directory>
```
Or, to solution
```
CodePorter.Applications.Console.exe -m p -s <path-to-sourse>\<name-of-solution>.sln -d <path-to-destination-directory>
```
Examples of use:

To port the solution, use: `CodePorting.Applications.Console.exe -m p -s \*.sln`

To simplify the solution, use: `CodePorting.Applications.Console.exe -m s -s \*.sln`

To analyze the solution, use: `CodePorting.Applications.Console.exe -m a -s \*.sln`

To save the result (txt or html formats are supported), use : `CodePorting.Applications.Console.exe -m a $targetFilePath.txt$ -s \*.sln`
