# Repositories

How does Gradle find the files for external dependencies? Gradle looks for them in a repository. A repository is really just a collection of files, organized by group, name and version. Gradle understands several different repository formats, such as Maven and Ivy, and several different ways of accessing the repository, such as using the local file system or HTTP.

By default, Gradle does not define any repositories. You need to define at least one before you can use external dependencies. One option is use the Maven central repository:

Example 8.4. Usage of Maven central repository

build.gradle

    repositories {
        mavenCentral()
    }

Or a remote Maven repository:

Example 8.5. Usage of a remote Maven repository

build.gradle

    repositories {
        maven {
            url "http://repo.mycompany.com/maven2"
        }
    }
Or a remote Ivy repository:

Example 8.6. Usage of a remote Ivy directory

build.gradle

    repositories {
        ivy {
            url "http://repo.mycompany.com/repo"
        }
    }

You can also have repositories on the local file system. This works for both Maven and Ivy repositories.

Example 8.7. Usage of a local Ivy directory

build.gradle

    repositories {
        ivy {
            // URL can refer to a local directory
            url "../local-repo"
        }
    }

A project can have multiple repositories. Gradle will look for a dependency in each repository in the order they are specified, stopping at the first repository that contains the requested module.

To find out more about defining and working with repositories, have a look at Section 50.6, “Repositories”.


