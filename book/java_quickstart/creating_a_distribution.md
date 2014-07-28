# Creating a distribution

We also add a distribution, that gets shipped to the client:

Example 7.14. Multi-project build - distribution file

api/build.gradle

    task dist(type: Zip) {
        dependsOn spiJar
        from 'src/dist'
        into('libs') {
            from spiJar.archivePath
            from configurations.runtime
        }
    }

    artifacts {
       archives dist
    }
