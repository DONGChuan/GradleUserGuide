# External dependencies

Usually, a Java project will have some dependencies on external JAR files. To reference these JAR files in the project, you need to tell Gradle where to find them. In Gradle, artifacts such as JAR files, are located in a repository. A repository can be used for fetching the dependencies of a project, or for publishing the artifacts of a project, or both. For this example, we will use the public Maven repository:

Example 7.3. Adding Maven repository

build.gradle

    repositories {
        mavenCentral()
    }

Let's add some dependencies. Here, we will declare that our production classes have a compile-time dependency on commons collections, and that our test classes have a compile-time dependency on junit:

Example 7.4. Adding dependencies

build.gradle

    dependencies {
        compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
        testCompile group: 'junit', name: 'junit', version: '4.+'
    }

You can find out more in Chapter 8, Dependency Management Basics.


