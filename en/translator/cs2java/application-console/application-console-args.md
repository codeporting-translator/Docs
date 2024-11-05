# Command line arguments of a console application

Click on CodePorting.Applications.Console.exe to get this console
to get help information about launching the CodePorting.translator Cs2Java console application, just launch the application with the`-h` switch. Open a cmd shell window and run the following command in it.
```
CodePorting.Applications.Console.exe -h
```
## The window will display basic information on using the Console application.
```
CodePorting.Applications.Console 24.7.0.0 ()
Available parameters:
    ['-help', '-h'] Help
    ['-conversionworkflow', '-wf'] Conversion workflow (A GUID of conversion workflow to run.)
    ['-sources', '-s'] Sources (Initial sources.)
    ['-destination', '-d'] Destination directory (The destination file path.)
    ['-loggerconfiguration', '-lc'] Logger configuration (The logger configuration file path.)
    ['-logginglevel', '-l'] Logging level
    ['-maxthreads', '-t'] Max degree of parallelism (Maximum parallel executing threads.)
    ['-quiet', '-q'] Quiet mode (Turned off logging.)
    ['-configuration', '-c'] Configuration (The configuration file path.)
    ['-workertask', '-wt'] Worker task id (Specifies setting profile id from the config file.)
    ['-about', '-a'] About
    ['-loadedassemblylist', '-la'] List of the loaded assemblies

Available extension parameters:
    Extension assembly name: CodePorting.Extensions.Applications.Tools.CompareAndSync.CompareAndSyncToolExtension
    ['-compareandsync', '-compareandsync(compareandsync)'] CompareAndSync (Compares 2 folders, merges files with diff and stores the diff files into output folder.)
    ['-oldfolderpath', '-oldfolderpath(compareandsync)'] Old folder (Folder with the old version of the java project)
    ['-newfolderpath', '-newfolderpath(compareandsync)'] New folder (Folder with the new version of the java project)
    ['-outputfolderpath', '-outputfolderpath(compareandsync)'] Result java folder (Folder with java project to which the merge will be applied)
    ['-syncfoldermappingspath', '-syncfoldermappingspath(compareandsync)'] Mappings Config File Path (Path to the file with folders mapping rules)

    Extension assembly name: CodePorting.Extensions.Applications.Tools.Analysis.AnalyzeToolExtension
    ['-analyze', '-analyze(analyze)'] analyze
    ['-sourcepaths', '-sourcepaths(analyze)'] Source paths (Source paths for analysis)
    ['-destinationpath', '-destinationpath(analyze)'] Destination path (The path for storing the analysis result)

    Extension assembly name: CodePorting.Extensions.Applications.Tools.TranslationAttributes.TranslationAttributesToolExtension
    ['-translationattributes', '-translationattributes(translationattributes)'] TranslationAttributes (Tool for storing attributes in config and code snippets in dedicated folder)
    ['-solutionpath', '-solutionpath(translationattributes)'] Solution path (Specify the path to the solution you are going to work with)

Available parameter prefixes: ['-', '/', '\']
Press any key to exit.
```
