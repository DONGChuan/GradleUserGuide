# 总结

下面是一个完整的构建文件的样本:

*Example 7.9. Java 例子 - 完整的构建文件*

*build.gradle*

    apply plugin: 'java'
    apply plugin: 'eclipse'

    sourceCompatibility = 1.5
    version = '1.0'
    jar {
        manifest {
            attributes 'Implementation-Title': 'Gradle Quickstart', 'Implementation-Version': version
        }
    }

    repositories {
        mavenCentral()
    }

    dependencies {
        compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
        testCompile group: 'junit', name: 'junit', version: '4.+'
    }

    test {
        systemProperties 'property': 'value'
    }

    uploadArchives {
        repositories {
           flatDir {
               dirs 'repos'
           }
        }
    }
