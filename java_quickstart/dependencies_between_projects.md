# 项目之间的依赖

你可以在同一个构建里加入项目之间的依赖, 举个例子, 一个项目的 JAR 文件被用来编译另外一个项目. 在 api 构建文件里我们将加入一个由 shared 项目产生的 JAR 文件的依赖. 由于这个依赖, Gradle 将确保 shared 项目总是在 api 之前被构建.

*Example 7.13. 多项目构建 - 项目之间的依赖*

*api/build.gradle*

    dependencies {
        compile project(':shared')
    }



