# CodePorting.Translator application configuration

The operation of CodePorting.Translator applications depends on the configuration parameters. Some of these parameters are the same for all applications. The configuration is represented as a set of levels that are sequentially executed when applications are loaded. The value received at the execution of the next level overwrites the previously set value. Not every level contains values for all parameters, therefore, after an application is fully loaded, some parameters may retain the value set when executing the earlier levels.

Below is a list of levels in the order of their execution sequence:

1. Default value level. These values are used to initialize the parameters in the application code, so, As soon as the parameter has appeared, it takes the value from this level.
1. Gobal value level. These values are stored somewhere in a publicly accessible place. For example, it can be a file in the “My Documents” folder or a branch in the operating system registry.
1. Application level. The values of this level are stored in a file placed in the application launch directory. This will allow each disk copy of the application to run with its own parameters.
1. Command line parameter level. Applications can be linked to the command line. In this case, parameter values can be passed when launching the application through command line arguments.
1. There is also the option of passing a separate configuration file to the command line parameters. This allows you to prepare several configuration files for each application and easily use any of them in each startup.
1. Dialog level. Applications that run in interactive modes, such as using any graphical interfaces, have the ability to provide the user with dialogs for editing application configuration parameters.

Each configuration option has a default value. So, if an option was not configured via configuration file it will have the default value.

**Important Note:**

Depending on the established policies for storing configuration parameters, some parameters may save their changed values at the global value level and then this value will be valid for all other applications.
