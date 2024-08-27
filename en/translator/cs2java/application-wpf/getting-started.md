## Getting started with GUI application (Windows)

###  Download release archive

[Download latest release here](https://products.codeporting.com/translator/csharp-to-java/release)

###  Extracting archive

Release archive contains two folders : `bin/console` and `bin/gui`.
For this tutorial only `bin/gui` folder is required.
1.  Extract that folder somewhere (`C:/translator_tutorial`, for example). It must be `C:/translator_tutorial/bin/gui` after extraction.
2.  Copy your license file to `C:/translator_tutorial` so that it is `C:/translator_tutorial/CodePorting.Translator.Cs2Java.lic`

### Downloading example project (optional)
Download [example project](https://github.com/codeporting-translator/codeporting-translator-cs2java) and extract it to the path `C:/translator_tutorial/` or clone it using **git** :
1. Run your command shell (it may be powershell)
2. Execute following commands :
```
cd C:/translator_tutorial
git clone https://github.com/codeporting-translator/codeporting-translator-cs2java
```
### Run gui executable file
1. Double-click on file `C:/translator_tutorial/bin/gui/CodePorting.Applications.WPF.exe`
2. After window is open, click "Actions" -> "Open wizard..."
3. Click "Next"
4. Select option "Continue without profile", then click "Next"
5. Select option "Translate C# code from Java", then press "Add path" to add C# source files. In this tutorial we choose file `C:/translator_tutorial/codeporting-translator-cs2java/ExampleProjects/TranslatorTestProject/TranslatorTestProject.sln`.
6. Click "Next"
7. Select output directory, clicking "..." button or entering directory path manually. Make sure, that path is valid and directory does exist.
8. Click "Finish" and wait for the translation process end

When translation is over, java files will be located in the output directory you selected in step 7.