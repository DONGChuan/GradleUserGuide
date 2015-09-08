## 22.8.Javadoc

Javadoc task 是 [Javadoc](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.javadoc.Javadoc.html) 的一个实例. 它支持 Javadoc 的核心选项和可执行的 Javadoc 的[reference documentation](http://download.oracle.com/javase/1.5.0/docs/tooldocs/windows/javadoc.html#referenceguide)中描述的标准JavaTOC的选项。有关支持Javadoc选项的完整列表，请参阅以下类的API文档：[CoreJavadocOptions](https://docs.gradle.org/current/javadoc/org/gradle/external/javadoc/CoreJavadocOptions.html)和[StandardJavadocDocletOptions](https://docs.gradle.org/current/javadoc/org/gradle/external/javadoc/StandardJavadocDocletOptions.html).

**表22.10.java插件-javadoc配置**

任务属性 | 类型 | 默认值
-------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------
classpath      | [FileCollection](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileCollection.html)                                                                                                                                                     | sourceSets.main.output + sourceSets.main.compileClasspath
source         | [FileTree](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileTree.html).可以设置为在[Section 15.5, “Specifying a set of input files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:specifying_multiple_files)中描述的任何值 | sourceSets.main.allJava
destinationDir | File                                                                                                                                                                                                                                                  | docsDir/javadoc
title          | String                                                                                                                                                                                                                                                | project的name和version
