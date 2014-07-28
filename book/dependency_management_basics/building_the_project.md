# Building the project
The Java plugin adds quite a few tasks to your project. However, there are only a handful of tasks that you will need to use to build the project. The most commonly used task is the build task, which does a full build of the project. When you run **gradle build**, Gradle will compile and test your code, and create a JAR file containing your main classes and resources:

Example 7.2. Building a Java project

Output of gradle build

    > gradle build
    :compileJava
    :processResources
    :classes
    :jar
    :assemble
    :compileTestJava
    :processTestResources
    :testClasses
    :test
    :check
    :build

BUILD SUCCESSFUL

Total time: 1 secs
Some other useful tasks are:

**clean**

Deletes the build directory, removing all built files.

**assemble**

Compiles and jars your code, but does not run the unit tests. Other plugins add more artifacts to this task. For example, if you use the War plugin, this task will also build the WAR file for your project.

**check**

Compiles and tests your code. Other plugins add more checks to this task. For example, if you use the checkstyle plugin, this task will also run Checkstyle against your source code.

