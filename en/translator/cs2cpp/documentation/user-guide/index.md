---
order: "2"
navTitle: "User Guide"
---

# CodePorting.Translator Cs2Cpp User Guide #

This section collects the primary user-facing documentation for the C# to C++ translator.

- [Command-line Interface](command-line-interface.md) - how to run and configure the translator from the CLI.
- [Manual Code Control](manual-code-control.md) - guidance on manual edits, annotations, and controlling generated code.
- [User-defined Exceptions](user-defined-exceptions.md) - handling and mapping exceptions between C# and C++.
- **Configuration File:**
  - [Attributes](configuration-file/attributes.md) - attribute settings available in the config file.
  - [Main](configuration-file/main.md) - top-level configuration and entry points.
  - [Nodes](configuration-file/nodes.md) - node-specific options and their meaning.
  - [Options](configuration-file/options.md) - misc configuration options and defaults.
- [C++ Attributes Reference](cpp-attributes/reference.md) - reference for C++-side attributes supported by the translator.
- **Integration:**
  - [CMake Support](integration/cmake-support.md) - building translated projects with CMake.
  - [Qt Support](integration/qt-support.md) - notes on Qt integration and special considerations.
  - [Supported Platforms](integration/supported-platforms.md) - platforms and environment caveats.
- [Limitations and Bugs](limitations-and-bugs/index.md) - known limitations, issues, and workarounds.

If you're looking for examples or tutorials, see the top-level `tutorial` folder.
