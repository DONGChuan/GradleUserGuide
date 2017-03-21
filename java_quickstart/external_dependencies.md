# 外部的依赖

通常,
一个 Java 项目的依赖许多外部的 JAR 文件.为了在项目里引用这些 JAR 文件,你需要告诉 Gradle 去哪里找它们.在 Gradle 中,JAR 文件位于一个仓库中，这里的仓库类似于 MAVEN 的仓库，可以被用来提取依赖,或者放入依赖。

举个例子，我们将使用开放的 Maven 仓库:

**例子 7.3. 加入 Maven 仓库**

*build.gradle*

    repositories {
        mavenCentral()
    }

接下来让我们加入一些依赖.
这里,
我们假设我们的项目在编译阶段有一些依赖:

*例子 6.4. 加入依赖*

*build.gradle*

    dependencies {
        compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
        testCompile group: 'junit', name: 'junit', version: '4.+'
    }

可以看到 commons-collections 被加入到了编译阶段,
junit 也被加入到了测试编译阶段.

你可以在第 8 章里看到更多这方面的内容.


