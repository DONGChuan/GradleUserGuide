# 使用资源设置

你可以通过 sourceSets 属性来使用资源设置. 它其实是一个项目资源设置的容器, 它的类型是 [SourceSetContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/SourceSetContainer.html). 同时也有 sourceSets { } 脚本模块, 在这个模块里, 你可以通过闭包配置资源设置容器. 资源设置容器的工作方式和其余容器几乎一模一样, 比如 tasks.

**例 22.3. 进入资源设置**

**build.gradle**

```
// Various ways to access the main source set
println sourceSets.main.output.classesDir
println sourceSets['main'].output.classesDir
sourceSets {
    println main.output.classesDir
}
sourceSets {
    main {
        println output.classesDir
    }
}

// Iterate over the source sets
sourceSets.all {
    println name
}
```

To configure an existing source set, you simply use one of the above access methods to set the properties of the source set. The properties are described below. Here is an example which configures the main Java and resources directories:

Example 22.4. Configuring the source directories of a source set

build.gradle
sourceSets {
    main {
        java {
            srcDir 'src/java'
        }
        resources {
            srcDir 'src/resources'
        }
    }
}

### 1. Source set properties

The following table lists some of the important properties of a source set. You can find more details in the API documentation for SourceSet.

Table 22.9. Java plugin - source set properties

Property name	Type	Default value	Description
name	String (read-only)	Not null	The name of the source set, used to identify it.
output	SourceSetOutput (read-only)	Not null	The output files of the source set, containing its compiled classes and resources.
output.classesDir	File	buildDir/classes/name	The directory to generate the classes of this source set into.
output.resourcesDir	File	buildDir/resources/name	The directory to generate the resources of this source set into.
compileClasspath	FileCollection	compileSourceSet configuration.	The classpath to use when compiling the source files of this source set.
runtimeClasspath	FileCollection	output + runtimeSourceSet configuration.	The classpath to use when executing the classes of this source set.
java	SourceDirectorySet (read-only)	Not null	The Java source files of this source set. Contains only .java files found in the Java source directories, and excludes all other files.
java.srcDirs	Set<File>. Can set using anything described in Section 15.5, “Specifying a set of input files”.	[projectDir/src/name/java]	The source directories containing the Java source files of this source set.
resources	SourceDirectorySet (read-only)	Not null	The resources of this source set. Contains only resources, and excludes any .java files found in the resource source directories. Other plugins, such as the Groovy plugin, exclude additional types of files from this collection.
resources.srcDirs	Set<File>. Can set using anything described in Section 15.5, “Specifying a set of input files”.	[projectDir/src/name/resources]	The source directories containing the resources of this source set.
allJava	SourceDirectorySet (read-only)	java	All .java files of this source set. Some plugins, such as the Groovy plugin, add additional Java source files to this collection.
allSource	SourceDirectorySet (read-only)	resources + java	All source files of this source set. This include all resource files and all Java source files. Some plugins, such as the Groovy plugin, add additional source files to this collection.
22.7.2. Defining new source sets

To define a new source set, you simply reference it in the sourceSets { } block. Here's an example:

Example 22.5. Defining a source set

build.gradle
sourceSets {
    intTest
}
When you define a new source set, the Java plugin adds some dependency configurations for the source set, as shown in Table 22.6, “Java plugin - source set dependency configurations”. You can use these configurations to define the compile and runtime dependencies of the source set.

Example 22.6. Defining source set dependencies

build.gradle
sourceSets {
    intTest
}

dependencies {
    intTestCompile 'junit:junit:4.12'
    intTestRuntime 'org.ow2.asm:asm-all:4.0'
}
The Java plugin also adds a number of tasks which assemble the classes for the source set, as shown in Table 22.2, “Java plugin - source set tasks”. For example, for a source set called intTest, compiling the classes for this source set is done by running gradle intTestClasses.

Example 22.7. Compiling a source set

Output of gradle intTestClasses
> gradle intTestClasses
:compileIntTestJava
:processIntTestResources
:intTestClasses

BUILD SUCCESSFUL

Total time: 1 secs
22.7.3. Some source set examples

Adding a JAR containing the classes of a source set:

Example 22.8. Assembling a JAR for a source set

build.gradle
task intTestJar(type: Jar) {
    from sourceSets.intTest.output
}
Generating Javadoc for a source set:

Example 22.9. Generating the Javadoc for a source set

build.gradle
task intTestJavadoc(type: Javadoc) {
    source sourceSets.intTest.allJava
}
Adding a test suite to run the tests in a source set:

Example 22.10. Running tests in a source set

build.gradle
task intTest(type: Test) {
    testClassesDir = sourceSets.intTest.output.classesDir
    classpath = sourceSets.intTest.runtimeClasspath
}
