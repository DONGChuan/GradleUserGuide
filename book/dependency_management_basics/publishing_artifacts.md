# Publishing artifacts
Dependency configurations are also used to publish files.[3] We call these files publication artifacts, or usually just artifacts.

The plugins do a pretty good job of defining the artifacts of a project, so you usually don't need to do anything special to tell Gradle what needs to be published. However, you do need to tell Gradle where to publish the artifacts. You do this by attaching repositories to the uploadArchives task. Here's an example of publishing to a remote Ivy repository:

Example 8.8. Publishing to an Ivy repository

build.gradle

    uploadArchives {
        repositories {
            ivy {
                credentials {
                    username "username"
                    password "pw"
                }
                url "http://repo.mycompany.com"
            }
        }
    }

Now, when you run gradle uploadArchives, Gradle will build and upload your Jar. Gradle will also generate and upload an ivy.xml as well.

You can also publish to Maven repositories. The syntax is slightly different.[4] Note that you also need to apply the Maven plugin in order to publish to a Maven repository. In this instance, Gradle will generate and upload a pom.xml.

Example 8.9. Publishing to a Maven repository

build.gradle

    apply plugin: 'maven'

    uploadArchives {
        repositories {
            mavenDeployer {
                repository(url: "file://localhost/tmp/myRepo/")
            }
        }
    }

To find out more about publication, have a look at Chapter 51, Publishing artifacts.


