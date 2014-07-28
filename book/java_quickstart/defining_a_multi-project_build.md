# Defining a multi-project build

To define a multi-project build, you need to create a settings file. The settings file lives in the root directory of the source tree, and specifies which projects to include in the build. It must be called settings.gradle. For this example, we are using a simple hierarchical layout. Here is the corresponding settings file:

Example 7.11. Multi-project build - settings.gradle file

settings.gradle

    include "shared", "api", "services:webservice", "services:shared"

You can find out more about the settings file in Chapter 56, Multi-project Builds.


