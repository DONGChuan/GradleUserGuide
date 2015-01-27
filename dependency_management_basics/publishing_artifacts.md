# 发布 artifacts
依赖配置也可以用来发布文件. 我们称这些文件publication artifacts, 或者就叫 artifacts.

插件可以很好的定义一个项目的 artifacts, 所以你并不需要做一些特别的工作来让 Gradle 需要发布什么. 你可以通过在 uploadArchives 任务里加入仓库来完成. 下面是一个发布远程 Ivy 库的例子:

*例子 7.8. 发布一个 Ivy 库*

*build.gradle*

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

现在, 当你运 **gradle uploadArchives**, Gradle 将构建和上传你的 Jar. Gradle 也会生成和上传 ivy.xml .

你也可以发布到 Maven 库. 请注意你需要加入 Maven 插件来发布一个 Maven 库. 在下面的例子里, Gradle 将生成和上传 pom.xml.

*例子 7.9. 发布 Maven 库*

*build.gradle*

    apply plugin: 'maven'

    uploadArchives {
        repositories {
            mavenDeployer {
                repository(url: "file://localhost/tmp/myRepo/")
            }
        }
    }

在第 51 章, 发布 artifacts 里有更加具体的介绍.


