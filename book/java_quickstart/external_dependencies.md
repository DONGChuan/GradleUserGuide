# 外部的依赖

通常, 一个 Java 项目将有许多外部的依赖, 既是指外部的 JAR 文件. 为了在项目里引用这些 JAR 文件, 你需要告诉 Gradle 去哪里找它们. 在 Gradle 中, JAR 文件位于一个仓库中，这里的仓库类似于 MAVEN 的仓库. 仓库可以被用来提取依赖, 或者放入一个依赖, 或者两者皆可. 举个例子, 我们将使用开放的 Maven 仓库:

*Example 7.3. 加入 Maven 仓库*

*build.gradle*

    repositories {
        mavenCentral()
    }

接下来让我们加入一些依赖. 这里, 我们假设我们的项目在编译阶段有一些依赖:

*Example 7.4. 加入依赖*

*build.gradle*

    dependencies {
        compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
        testCompile group: 'junit', name: 'junit', version: '4.+'
    }

你可以在第7章里看到更多这方面的内容.


