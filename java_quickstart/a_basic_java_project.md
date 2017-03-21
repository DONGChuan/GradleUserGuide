# 一个基础的 Java 项目

让我们先来看一个简单的例子.

我们可以加入下面的代码来使用 Java 插件:

**例子 7.1. 使用 Java 插件**

_build.gradle_

```
apply plugin: 'java'
```

（注：此例子的代码可以再所有“-all”结尾的发行版的samples/java/quickstart目录下找到）

它将会把 Java 插件加入到你的项目中,  这意味着许多预定制的任务会被自动加入到你的项目里.

Gradle 默认在 **src/main/java** 目录下寻找到你的正式（生产）源码, 在 **src/test/java** 目录下寻找到你的测试源码, 并在**src/main/resources**目录下寻找到你的  
也就是说 Gradle 默认地在这些路径里查找资源.  
另外,  
任何在 src/main/resources 的文件都将被包含在 JAR 文件里,  
同时任何在 src/test/resources 的文件会被加入到 classpath 中以运行测试代码. 所有的输出文件将会被创建在构建目录里,  
JAR 文件存放在 build/libs 文件夹里.

**都有什么可以执行的任务呢?**

你可以使用 **gradle tasks** 来列出项目的所有任务.  
通过这个命令来看看 Java 插件都在你的项目里加入了哪些命令吧.

