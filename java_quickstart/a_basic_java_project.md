# 一个基础的 Java 项目

让我们来看一个简单的例子. 加入下面的代码来使用 Java 插件:

*例子 6.1. 使用 Java 插件*

*build.gradle*

    apply plugin: 'java'

这个例子的代码可以在 samples/java/quickstart 里找到, 二进制代码和源代码里都包含这些文件.
它将会把 Java 插件加入到你的项目中, 这意味着许多任务被自动的加入到了你的项目里.

Gradle 希望能在 src/main/java 找到你的源代码, 在 src/test/java 找到你的测试代码. 另外, 任何在 src/main/resources 的文件都将被包含在 JAR 文件里, 同时任何在 src/test/resources 的文件会被加入到  classpath 中以运行测试代码. 所有的输出文件将会被创建在构建目录里,  JAR 文件 存放在 build/libs 文件夹里.

**都有什么可以执行的任务呢?**

你可以使用 ****gradle tasks** 来列出项目的所有任务to. 通过这个命令来尝试看看 Java 插件都在你的项目里加入了哪些命令吧.


