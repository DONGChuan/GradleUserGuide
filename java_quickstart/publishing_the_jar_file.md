# 发布 JAR 文件

通常 JAR 文件需要在某个地方发布. 为了完成这一步, 你需要告诉 Gradle 哪里发布 JAR 文件. 在 Gradle 里, 生成的文件比如 JAR 文件将被发布到仓库里. 在我们的例子里, 我们将发布到一个本地的目录. 你也可以发布到一个或多个远程的地点.

*Example 7.7. 发布 JAR 文件*

*build.gradle*

    uploadArchives {
        repositories {
           flatDir {
               dirs 'repos'
           }
        }
    }

运行 **gradle uploadArchives** 命令来发布 JAR 文件.


