## 22.11.编译java
java插件为项目的每一个source set增加了一个[JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html)实例,最常见的配置选项如下所示:

**表22.13.java插件-编译配置**

任务属性 | 类型 | 默认值
-------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------
classpath | [FileCollection](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileCollection.html) | sourceSet.compileClasspath
source | [FileTree](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileTree.html),可以在[Section 15.6, “Copying files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:copying_files)中查看可以设置什么. | sourceSet.java
destinationDir | File.| sourceSet.output.classesDir

默认情况下 java 的编译运行在 Gradle 中的进程. 设置 option.fork 为 true 会使编译在一个单独的进程中运行,在Ant中运行 javac任务意味着一个新进程将被拆封为多个编译任务，这会减慢编译。相反的,Gradle的直接编译集成（见上文）在编译过程中将尽可能地重复使用相同的进程.在所有情况下由options.forkOptions指定的选项会被实现.
