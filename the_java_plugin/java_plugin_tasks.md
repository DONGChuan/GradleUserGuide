# 任务

Java 插件引入了许多任务到项目当中, 具体如下表所示

**表22.1 java 插件-任务**

任务名     | 依赖        |  类型 | 描述
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
uploadArchives | 在archives配置中产生信息单元的文件，包括了 jar | [Upload](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Upload.html) | 上传信息单元在archives配置中，包括 Jar 文件
clean | - | [Delete](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Delete.html) | 删除项目构建目录
clean*TaskName* | - | [Delete](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Delete.html) | 删除指定任务名所产生的项目构建目录，CleanJar会删除jar任务创建的jar 文件，cleanTest将会删除由 test 任务创建的测试结果

对于添加到项目中的每个资源设置, java 插件将会加入以下编译任务

**表22.2.java 插件-资源设置任务**

任务名 | 依赖 | 类型 | 描述
--------- | ---------- | ---- | -----------
compile*SourceSet*Java | 产生资源设置编译 classpath 的所有任务 | [JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.Configuration.html) | 使用 javac 命令编译给定资源设置的 Java 源文件
processSourceSetResources | - | [Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html) | 复制给定资源设置的资源到classes目录下。
sourceSetClasses | compileSourceSetJava任务和processSourceSetResources任务。一些插件给资源设置添加额外的编译工作。 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 组装资源设置的class目录

Java 插件同时也增加了一些为项目生命周期服务的任务

**表22.3.java 插件-生命周期任务**

任务名 | 依赖 | 类型 | 描述
--------- | ---------- | ---- | -----------
assemble | 项目中的所有归档任务，包括 jar 任务。一些插件给项目增加的额外归档任务 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 组装项目的所有档案
check | 项目中的所有验证任务，包括 test 任务。一些插件给项目增加的额外验证任务 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 执行项目中的所有验证任务
build | assemble任务和 check 任务 | | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 构建完整地项目
buildNeeded | build 任务和buildNeeded 任务的testRuntime任务配置的所有项目的依赖库 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 构建完整地项目并且构建该项目依赖的所有项目
buildDependents | build and buildDependents tasks in all projects with a project lib dependency on this project in a testRuntime configuration. | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 构建完整项目并且构建所有依赖该项目的项目
buildConfigName  | 产生由ConfigName配置的信息单元的任务。 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 根据指定的配置组装信息单元。这个任务是由 Java 插件隐式添加的基础插件添加的。
uploadConfigName | 上传由ConfigName配置的信息单元的任务。 | [Upload](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Upload.html) | 根据指定的配置组装并上传信息单元。
。这个任务是由 Java 插件隐式添加的基础插件添加的。

下图显示了这些任务之间的关系

**图22.1.java 插件-任务**

![java plugin-tasks](https://docs.gradle.org/current/userguide/img/javaPluginTasks.png)
