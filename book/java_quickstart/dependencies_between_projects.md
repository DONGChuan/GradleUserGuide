# Dependencies between projects

You can add dependencies between projects in the same build, so that, for example, the JAR file of one project is used to compile another project. In the api build file we will add a dependency on the JAR produced by the shared project. Due to this dependency, Gradle will ensure that project shared always gets built before project api.

Example 7.13. Multi-project build - dependencies between projects

api/build.gradle

    dependencies {
        compile project(':shared')
    }

See Section 56.7.1, “Disabling the build of dependency projects” for how to disable this functionality.

