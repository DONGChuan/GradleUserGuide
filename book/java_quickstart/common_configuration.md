# Common configuration

For most multi-project builds, there is some configuration which is common to all projects. In our sample, we will define this common configuration in the root project, using a technique called configuration injection. Here, the root project is like a container and the subprojects method iterates over the elements of this container - the projects in this instance - and injects the specified configuration. This way we can easily define the manifest content for all archives, and some common dependencies:

Example 7.12. Multi-project build - common configuration

build.gradle
    subprojects {
        apply plugin: 'java'
        apply plugin: 'eclipse-wtp'

        repositories {
           mavenCentral()
        }

        dependencies {
            testCompile 'junit:junit:4.11'
        }

        version = '1.0'

        jar {
            manifest.attributes provider: 'gradle'
        }
    }
Notice that our sample applies the Java plugin to each subproject. This means the tasks and configuration properties we have seen in the previous section are available in each subproject. So, you can compile, test, and JAR all the projects by running gradle build from the root project directory.


