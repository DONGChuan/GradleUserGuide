# A basic Java project

Let's look at a simple example. To use the Java plugin, add the following to your build file:

Example 7.1. Using the Java plugin

build.gradle

    apply plugin: 'java'

Note: The code for this example can be found at samples/java/quickstart which is in both the binary and source distributions of Gradle.
This is all you need to define a Java project. This will apply the Java plugin to your project, which adds a number of tasks to your project.

What tasks are available?

You can use gradle tasks to list the tasks of a project. This will let you see the tasks that the Java plugin has added to your project.
Gradle expects to find your production source code under src/main/java and your test source code under src/test/java. In addition, any files under src/main/resources will be included in the JAR file as resources, and any files under src/test/resources will be included in the classpath used to run the tests. All output files are created under the build directory, with the JAR file ending up in the build/libs directory.


