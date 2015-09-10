# 仓库

Gradle 是怎样找到那些外部依赖的文件的呢? Gradle 会在一个*repository(仓库)*里找这些文件. 仓库其实就是文件的集合, 通过 `group`, `name` 和 `version` 整理分类. Gradle 能解析好几种不同的仓库形式, 比如 Maven 和 Ivy, 同时可以理解各种进入仓库的方法, 比如使用本地文件系统或者 HTTP.

默认地, Gradle 不提前定义任何仓库. 在使用外部依赖之前, 你需要自己至少定义一个库. 比如使用下面例子中的 Maven central 仓库:

*例子 8.4. Maven central 仓库*

*build.gradle*

    repositories {
        mavenCentral()
    }

或者使用一个远程的 Maven 仓库:

*例子 8.5. 使用远程的 Maven 仓库*

*build.gradle*

    repositories {
        maven {
            url "http://repo.mycompany.com/maven2"
        }
    }

或者一个远程的 Ivy 仓库:

*例子 8.6. 使用远程的 Ivy 仓库*

*build.gradle*

    repositories {
        ivy {
            url "http://repo.mycompany.com/repo"
        }
    }

你也可以使用本地的文件系统里的库. Mqven 和 Ivy 都支持下载的本地.

*例子 8.7. 使用本地的 Ivy 目录*

build.gradle

    repositories {
        ivy {
            // URL can refer to a local directory
            url "../local-repo"
        }
    }

一个项目可以有好几个库. Gradle 会根据依赖定义的顺序在各个库里寻找它们, 在第一个库里找到了就不会再在第二个库里找它了.

可以在[Section 50.6 章, “仓库”](https://docs.gradle.org/current/userguide/dependency_management.html#sec:repositories)里找到更详细的信息.


