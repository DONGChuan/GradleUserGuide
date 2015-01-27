# 构建一个 WAR 文件

为了构建一个 WAR 文件, 需要在项目中加入 War 插件:

*例子 9.1. War 插件*

*build.gradle*

    apply plugin: 'war'

这个插件也会在你的项目里加入 Java 插件. 运行 **gradle build** 将会编译, 测试和创建项目的 WAR 文件. Gradle 将会把源文件包含在 WAR 文件的 src/main/webapp 目录里. 编译后的 classe , 和它们运行所需要的依赖也会被包含在 WAR 文件里.
