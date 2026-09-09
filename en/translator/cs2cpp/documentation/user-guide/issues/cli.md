# Command Line Interface Issues #

This section describes issues that arise when writing the translator command line.

[TOC]

## CLI01: Invalid command line option {#cli01} ##

Occurs when an option used on the command line is not from the list of [current](../command-line-interface.md#supported-options) or [obsolete](../command-line-interface.md#obsolete-options) options.

**Default severity**: ERROR

**Example**: invalid `-cfg` option

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -cfg project.config
```

**Solution**: fix or remove invalid option from command line.

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -c project.config
```

## CLI02: Obsolete command line option {#cli02} ##

Occurs when an deprecated option used from the list of [obsolete](../command-line-interface.md#obsolete-options) options.

**Default severity**: WARNING

**Example**: deprecated `-q` option

```txt
CodeTranslator.Cs2Cpp.Console.exe -q source.csproj
```

**Solution**: remove deprecated option from command line.

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj
```

## CLI03: Duplicate command line option {#cli03} ##

Occurs when an option that can only be used once is specified twice or more.

**Default severity**: ERROR

**Example**: duplicate `-ct` option

```txt
CodeTranslator.Cs2Cpp.Console.exe -ct source.csproj -ct
```

**Solution**: remove duplicate option from command line.

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -ct
```

## CLI10: Bad option assignment {#cli10} ##

Occurs when overriding a configuration option via the `-o` switch has invalid syntax.

**Default severity**: ERROR

**Example**: forgotten '=' sign

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -o force_const_ref_parameters true
```

**Solution**: fix assignment syntax: `name=value`

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -o force_const_ref_parameters=true
```

## CLI11: The log level is not supported, the default value 'Info' will be used instead {#cli11} ##

Occurs when new log level after of log level with `-ll` switch doesn't match available values: Trace, Debug, Info, Warn, Error, Fatal, Off.

**Default severity**: WARNING

**Example**: invalid log level constant

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -ll Silent
```

**Solution**: fix log level constant to available value

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -ll Off
```

## CLI12: The severity rule has invalid format {#cli12} ##

Occurs when severity rule `-sr` in command line has invalid syntax.

**Default severity**: ERROR

**Example**: extra space in the issue identifier

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -sr cli 12 Fatal
```

**Solution**: remove extra symbols, use only valid constants

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj -sr cli12 Fatal
```
