# Building a WAR file

To build a WAR file, you apply the War plugin to your project:

Example 10.1. War plugin

build.gradle

    apply plugin: 'war'

Note: The code for this example can be found at samples/webApplication/quickstart which is in both the binary and source distributions of Gradle.
This also applies the Java plugin to your project. Running gradle build will compile, test and WAR your project. Gradle will look for the source files to include in the WAR file in src/main/webapp. Your compiled classes, and their runtime dependencies are also included in the WAR file
