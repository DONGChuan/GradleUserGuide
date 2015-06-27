# 22章 Java插件
Java插件给一个项目增加了编译，测试以及绑定Java的能力，许多 Gradle 的插件都需要以 Java 插件为基础
## 22.1.用法
要使用 Java 脚本，在构建脚本中加入如下内容

**例子 22.1.使用 Java 插件**

**bulid.gradle**

```
apply plugin: 'java'
```

## 22.2.源集
Java 插件引入了*_源集(Source Set)_*的概念，源集就是一组编译和执行的文件。
这些源文件可能包含 Java 的源文件以及一些资源文件。
其他的插件可能还会在源集中包含Groovy 和Scala的源文件,源集有一个与之关联的编译classpath和运行classpath。

使用源集的目的是为了将源文件根据它们的目的去归档到不同的逻辑组，举个例子，你可以使用一个源集来定义一个集成测试套件
也可以使用另外的源组来定义你项目的 API 和实现类。

Java 插件定义了两个标准源集，分别为`main`和`test`,`main`源集中包含了生产代码并且会编译并组装成 Jar 文件，
`test`源集包括了测试代码并且会使用JUnit或者TestNG编译和执行。它们可以是单元测试，集成测试，验收测试或者任何对你有用的测试集。

## 22.3.任务
引入 Java 插件增加了许多任务到项目，具体如下表所示

**表22.1 java 插件-任务**

任务名 | 依赖 | 类型 | 描述
--------- | ---------- | ---- | -----------
compileJava | 所有产生编译 classpath 的任务，包括编译配置项目的所依赖的 jar 文件 | [JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.Configuration.html) | 使用 javac 命令编译产生 java源文件
processResources | - | [Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html) | 复制生产资源到生产 class 文件目录
classes | compileJava任务和processResources任务。有一些插件添加额外的编译任务 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 组装生产class文件目录
compileTestJava | compile任务加上所有产生测试编译的classpath的任务 | [JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.Configuration.html) | 使用 javac编译产生 java 测试源文件
processTestResources | - | [Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html) | 复制测试资源到测试 class 文件目录
testClasses | compileTestJava 和 processTestResources 任务。一些插件会添加额外的测试编译任务 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 组装测试class文件目录
jar | compile | [Jar](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html) | 组装 Jar 文件
javadoc | compile | [javadoc](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.javadoc.Javadoc.html) | 使用 javadoc 命令为 Java 源码生产 API 文档
test | compile，compileTest，加上所有产生 test runtime classp 的任务 | [Test](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.testing.Test.html) | 使用 JUnit或者TestNG 进行单元测试
uploadArchives | 在archives配置中产生结果的文件，包括了 jar | [Upload](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Upload.html) | 上传结果在archives配置中，包括 Jar 文件
clean | - | [Clean](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Delete.html) | 删除项目构建目录
clean*TaskName* | - | [Clean](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Delete.html) | 删除指定任务名所产生的项目构建目录，CleanJar会删除jar任务创建的jar 文件，cleanTest将会删除由 test 任务创建的测试结果

对于添加到项目中的每个每个源集，java 插件加入了以下编译任务
