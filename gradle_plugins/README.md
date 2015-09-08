# Gradle 插件

Gradle 的核心为真实世界提供了很少的自动化. 
所有的实用特性,类似编译java源码的能力, 是由*插件*提供的. 插件添加了新的任务(如:[JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html)),域对象(如:[SourceSet](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.SourceSet.html)),公约(如:Java资源位置是`src/main/java`)以及来自其他插件延伸核心对象和对象。

在本章中，我们将讨论如何使用插件和关于插件的周边概念和术语。
