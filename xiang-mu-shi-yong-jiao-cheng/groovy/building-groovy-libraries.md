This guide demonstrates how to create a[Groovy](http://groovy-lang.org/)library in the standard form using Gradle’s Build Init plugin.

## [What you’ll need](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#what_you_ll_need) {#what_you_ll_need}

* About8 minutes

* A text editor

* A command prompt

* The Java Development Kit \(JDK\), version 1.7 or higher

* A[Gradle distribution](https://gradle.org/install), version 4.6 or better

## [Check the user manual](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#check_the_user_manual) {#check_the_user_manual}

Gradle comes with a built-in plugin called the Build Init plugin. It is documented in the[Gradle User Manual](https://docs.gradle.org/current/userguide/build_init_plugin.html). The plugin has one task, called`init`, that generates the project. The`init`task calls the \(also built-in\)`wrapper`task to create a Gradle wrapper script,`gradlew`.

The first step is to create a folder for the new project and change directory into it.

```
$ mkdir building-groovy-libraries
$ cd building-groovy-libraries
```

## [Run the init task](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#run_the_init_task) {#run_the_init_task}

From inside the new project directory, run the`init`task with the`groovy-library`argument.

```
$ gradle init --type groovy-library
:wrapper
:init

BUILD SUCCESSFUL in 4s
2 actionable tasks: 2 executed
```

The`init`task runs the`wrapper`task first, which generates the`gradlew`and`gradlew.bat`wrapper scripts. Then it creates the new project with the following structure:

```
├── build.gradle
├── gradle
│   └── wrapper  

│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   └── groovy  

    │       └── Library.groovy
    └── test
        └── groovy 

            └── LibraryTest.groovy
```

|  | Generated folder for wrapper files |
| :--- | :--- |
|  | Default Groovy source folder |
|  | Default Groovy test folder |

## [Review the generated project files](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#review_the_generated_project_files) {#review_the_generated_project_files}

The`settings.gradle`file is heavily commented, but has only one active line:

Generated settings.gradle

```
rootProject.name = 'gradleRunner'
```

|  | This assigns the name of the root project. |
| :--- | :--- |


The generated`build.gradle`file also has many comments. The active portion is reproduced here \(note version numbers for the dependencies may be updated in later versions of Gradle\):

Generated build.gradle

```
plugins {
    id 'groovy'
}

dependencies {
    compile 'org.codehaus.groovy:groovy-all:2.4.13' 

    testCompile 'org.spockframework:spock-core:1.0-groovy-2.4' 
}

repositories {
    jcenter() 
}
```

|  | Public Bintray Artifactory repository |
| :--- | :--- |
|  | Groovy version that this project will include as dependency |
|  | Spock Framework testing library |
|  | JUnit testing library |

The build file adds the`groovy`plugin. This is an extension of the`java`plugins and adds additional tasks for compiling Groovy source code. It also allows for joint compilation of Groovy and Java files if found in the appropriate`groovy`source folders.

The file`src/main/groovy/Library.groovy`is shown here:

Generated src/main/groovy/Library.groovy

```
class Library {
    boolean someLibraryMethod() {
        true
    }
}
```

The generated Spock specification,`src/test/groovy/LibraryTest.groovy`is shown next:

Generated src/test/groovy/LibraryTest.groovy

```
import spock.lang.Specification

class LibraryTest extends Specification {
    def "someLibraryMethod returns true"() {
        setup:
        def lib = new Library()

        when:
        def result = lib.someLibraryMethod()

        then:
        result == true
    }
}
```

The generated test class has a single[Spock specification](http://spockframework.org/). The test instantiates the`Library`class, invokes the`someLibraryMethod`method, and checks that the returned value is`true`.

## [Execute the build](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#execute_the_build) {#execute_the_build}

To build the project, run the`build`task. You can use the regular`gradle`command, but when a project includes a wrapper script, it is considered good form to use it instead.

```
$ ./gradlew build
:compileJava NO-SOURCE
:compileGroovy
:processResources NO-SOURCE
:classes
:jar
:assemble
:compileTestJava NO-SOURCE
:compileTestGroovy
:processTestResources NO-SOURCE
:testClasses
:test
:check
:build

BUILD SUCCESSFUL in 8s
4 actionable tasks: 4 executed
```

|  | The first time you run the wrapper script,`gradlew`, there may be a delay while that version of`gradle`is downloaded and stored locally in your`~/.gradle/wrapper/dists`folder. |
| :--- | :--- |


The first time you run the build, Gradle will check whether or not you already have the Groovy, Spock Framework and JUnit libraries in your cache under your`~/.gradle`directory. If not, the libraries will be downloaded and stored there. The next time you run the build, the cached versions will be used. The`build`task compiles the classes, runs the tests, and generates a test report.

You can view the test report by opening the HTML output file, located at`build/reports/tests/test/index.html`.

A sample report is shown here:

![](https://guides.gradle.org/building-groovy-libraries/images/Test-Summary.png "Test Summary")

Congratulations, you have just completed the step from creating a Groovy library! You can can now customise this to your own project needs.

## [Adding API documentation](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#adding_api_documentation) {#adding_api_documentation}

The`groovy`plugin has built-in support for Groovy’s API documentation tool`groovydoc`.

Annotate the`Library.groovy`file with Groovydoc markup.

src/main/groovy/Library.groovy

```
/** This Groovy source file was generated by the Gradle 'init' task.
 */
class Library {
    boolean someLibraryMethod() {
        true
    }
}
```

Run the`groovydoc`task.

```
$ ./gradlew groovydoc
:compileJava NO-SOURCE
:compileGroovy
:processResources NO-SOURCE
:classes
:groovydoc
[ant:groovydoc] /home/travis/build/gradle-guides/building-groovy-libraries/build/runners/gradleRunner/build/tmp/groovydoc contains source files in the default package, you must specify them as source files not packages.

BUILD SUCCESSFUL in 5s
2 actionable tasks: 2 executed
```

You can view the generated Groovydoc by opening the HTML file, located at`build/docs/groovydoc/index.html`.

## [Summary](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#summary) {#summary}

You now have a new Groovy project that you generated using Gradle’s build init plugin. In the process, you saw:

* How to generate a Groovy library

* How the generated build file and sample Groovy files are structured

* How to run the build and view the test report

* Generating API documentation.

## [Next Steps](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#next_steps) {#next_steps}

* Read about the[Groovy Plugin](https://docs.gradle.org/4.6/userguide/groovy_plugin.html)

* Get to know the configurations for the[GroovyCompile](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.compile.GroovyCompile.html)and[Groovydoc](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.javadoc.Groovydoc.html)task types.

## [Help improve this guide](https://guides.gradle.org/building-groovy-libraries/?_ga=2.20469688.1895311821.1524060450-1807434561.1523282416#help_improve_this_guide) {#help_improve_this_guide}

Have feedback or a question? Found a typo? Like all Gradle guides, help is just a GitHub issue away. Please add an issue or pull request to[gradle-guides/building-groovy-libraries](https://github.com/gradle-guides/building-groovy-libraries/)and we’ll get back to you.

