# Publishing the JAR file

Usually the JAR file needs to be published somewhere. To do this, you need to tell Gradle where to publish the JAR file. In Gradle, artifacts such as JAR files are published to repositories. In our sample, we will publish to a local directory. You can also publish to a remote location, or multiple locations.

Example 7.7. Publishing the JAR file

build.gradle

    uploadArchives {
        repositories {
           flatDir {
               dirs 'repos'
           }
        }
    }

To publish the JAR file, run gradle uploadArchives.


