# External dependencies

There are various types of dependencies that you can declare. One such type is an external dependency. This a dependency on some files built outside the current build, and stored in a repository of some kind, such as Maven central, or a corporate Maven or Ivy repository, or a directory in the local file system.

To define an external dependency, you add it to a dependency configuration:

Example 8.2. Definition of an external dependency

build.gradle

    dependencies {
        compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'
    }

An external dependency is identified using group, name and version attributes. Depending on which kind of repository you are using, group and version may be optional.

There is a shortcut form for declaring external dependencies, which uses a string of the form "group:name:version".

Example 8.3. Shortcut definition of an external dependency

build.gradle

    dependencies {
        compile 'org.hibernate:hibernate-core:3.6.7.Final'
    }

To find out more about defining and working with dependencies, have a look at Section 50.4, “How to declare your dependencies”.


