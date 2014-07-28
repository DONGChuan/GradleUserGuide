# customizing the project

The Java plugin adds a number of properties to your project. These properties have default values which are usually sufficient to get started. It's easy to change these values if they don't suit. Let's look at this for our sample. Here we will specify the version number for our Java project, along with the Java version our source is written in. We also add some attributes to the JAR manifest.

Example 7.5. Customization of MANIFEST.MF

build.gradle

    sourceCompatibility = 1.5
    version = '1.0'
    jar {
        manifest {
            attributes 'Implementation-Title': 'Gradle Quickstart', 'Implementation-Version': version
        }
    }

What properties are available?

You can use gradle properties to list the properties of a project. This will allow you to see the properties added by the Java plugin, and their default values.
The tasks which the Java plugin adds are regular tasks, exactly the same as if they were declared in the build file. This means you can use any of the mechanisms shown in earlier chapters to customize these tasks. For example, you can set the properties of a task, add behaviour to a task, change the dependencies of a task, or replace a task entirely. In our sample, we will configure the test task, which is of type Test, to add a system property when the tests are executed:

Example 7.6. Adding a test system property

build.gradle

    test {
        systemProperties 'property': 'value'
    }

