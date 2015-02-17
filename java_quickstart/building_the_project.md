# 建立项目

Java 插件在你的项目里加入了许多任务.
然而,
你只会用到其中的一小部分任务.
最常用的任务是 build 任务,
它会建立你的项目.
当你运行 **gradle build** 命令时,
Gradle 将会编译和测试你的代码,
并且创建一个包含类和资源的 JAR 文件:

**例子 7.2. 建立一个 Java 项目**

**gradle build** 命令的输出

    > gradle build
    :compileJava
    :processResources
    :classes
    :jar
    :assemble
    :compileTestJava
    :processTestResources
    :testClasses
    :test
    :check
    :build

    BUILD SUCCESSFUL

    Total time: 1 secs

其余一些有用的任务是:

**clean**

删除 build 生成的目录和所有生成的文件.

**assemble**

编译并打包你的代码,
但是并不运行单元测试.
其他插件会在这个任务里加入更多的东西.
举个例子,
如果你使用 War 插件,
这个任务将根据你的项目生成一个 WAR 文件.

**check**

编译并测试你的代码.
其他的插件会加入更多的检查步骤.
举个例子,
如果你使用 checkstyle 插件,
这个任务将会运行 Checkstyle 来检查你的代码.


