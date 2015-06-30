# 22章 Java插件
Java插件给一个项目增加了编译，测试以及绑定Java的能力，许多 Gradle 的插件都需要以 Java 插件为基础
## 22.1.用法
要使用 Java 脚本，在构建脚本中加入如下内容

**例子 22.1.使用 Java 插件**

**bulid.gradle**

```
apply plugin: 'java'
```

## 22.2.sourceSet
Java 插件引入了*_Source Set_*的概念，sourceSet就是一组编译和执行的文件。
这些源文件可能包含 Java 的源文件以及一些资源文件。
其他的插件可能还会在sourceSet中包含Groovy 和Scala的源文件,sourceSet有一个与之关联的编译classpath和运行classpath。

使用sourceSet的目的是为了将源文件根据它们的目的去归档到不同的逻辑组，举个例子，你可以使用一个sourceSet来定义一个集成测试套件
也可以使用另外的源组来定义你项目的 API 和实现类。

Java 插件定义了两个标准sourceSet，分别为`main`和`test`,`main`sourceSet中包含了生产代码并且会编译并组装成 Jar 文件，
`test`sourceSet包括了测试代码并且会使用JUnit或者TestNG编译和执行。它们可以是单元测试，集成测试，验收测试或者任何对你有用的测试集。

## 22.3.任务
引入 Java 插件增加了许多任务到项目，具体如下表所示

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
clean | - | [Clean](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Delete.html) | 删除项目构建目录
clean*TaskName* | - | [Clean](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Delete.html) | 删除指定任务名所产生的项目构建目录，CleanJar会删除jar任务创建的jar 文件，cleanTest将会删除由 test 任务创建的测试结果

对于添加到项目中的每个每个sourceSet，java 插件加入了以下编译任务

**表22.2.java 插件-sourceSet任务**

任务名 | 依赖 | 类型 | 描述
--------- | ---------- | ---- | -----------
compile*SourceSet*Java | 产生sourceSet编译 classpath 的所有任务 | [JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.Configuration.html) | 使用 javac 命令编译给定sourceSet的 Java 源文件
processSourceSetResources | - | [Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html) | 复制给定sourceSet的资源到classes目录下。
sourceSetClasses | compileSourceSetJava任务和processSourceSetResources任务。一些插件给sourceSet添加额外的编译工作。 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 组装sourceSet的class目录

Java 插件和增加了一些为项目构建生命周期的任务

**表22.3.java 插件-生命周期任务**

任务名 | 依赖 | 类型 | 描述
--------- | ---------- | ---- | -----------
assemble | 项目中的所有归档任务，包括 jar 任务。一些插件给项目增加的额外归档任务 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 组装项目的所有档案
check | 项目中的所有验证任务，包括 test 任务。一些插件给项目增加的额外验证任务 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 执行项目中的所有验证任务
build | assemble任务和 check 任务 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 构建完整地项目
buildNeeded | build 任务和buildNeeded 任务的testRuntime任务配置的所有项目的依赖库 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 构建完整地项目并且构建该项目依赖的所有项目
buildDependents | build and buildDependents tasks in all projects with a project lib dependency on this project in a testRuntime configuration. | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 构建完整项目并且构建所有依赖该项目的项目
buildConfigName  | 产生由ConfigName配置的信息单元的任务。 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 根据指定的配置组装信息单元。这个任务是由 Java 插件隐式添加的基础插件添加的。
uploadConfigName | 上传由ConfigName配置的信息单元的任务。 | [Upload](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Upload.html) | 根据指定的配置组装并上传信息单元。
。这个任务是由 Java 插件隐式添加的基础插件添加的。

下图显示了这些任务之间的关系

**图22.1.java 插件-任务**

![java plugin-tasks](https://docs.gradle.org/current/userguide/img/javaPluginTasks.png)

## 22.4.项目布局
Java 插件的默认布局如下图所示，无论这些文件夹中有没有内容，Java插件都会编译里面的内容，并处理缺失的内容。
**表22.4.java 插件-默认布局**

目录 | 含义
--- | ---
src/main/java |  主要 Java 源码
src/main/resources | 主要资源
src/test/java | 测试 Java 源码
src/test/resources | 测试资源
src/sourceSet/java | 指定sourceSet的 Java 源码
src/sourceSet/resources | 指定sourceSet的资源

### 22.4.1.改变项目布局
可以通过配置适当的源组配置项目布局，更多细节会在后面的章节中讨论，下面是一个配置了 java 和 resource 的简单的例子

**例22.2.自定义 Java 源码布局**

**build.gradle**

```
sourceSet{
  main{
    java{
      srcDir 'src/java'
    }
    resources{
      srcDir 'src/resources'
    }
  }
}
```

## 22.5.依赖管理
Java插件给project增加了相关配置，如下所示，这些配置被分配成像compileJava和test等配置

**表22.5.Java插件-依赖配置**

名称 | 扩展 | 运行任务 | 含义
--------- | ---------- | ---- | -----------
compile | - | compileJava | 编译时的依赖
runtime | compile | - | 运行时的依赖
testCompile | compile | compileTestJava | 编译测试所需的额外依赖
testRuntime | runtime | test | 仅供运行测试的额外依赖
archives | - | uploadArchives | 项目产生的信息单元（如:jar包）
default | runtime | - | 使用其他项目的默认依赖项，包括该项目产生的信息单元以及依赖

**图22.2.Java插件-依赖配置**
![java plugin-dependency configurations](https://docs.gradle.org/current/userguide/img/javaPluginConfigurations.png)

对于每个添加到该项目的sourceSet，java插件会添加一下的依赖配置

**表22.6.Java插件-sourceSet依赖关系配置**

名称 | 扩展 | 被使用时运行的任务 | 含义
--------- | ---------- | ---- | -----------
sourceSetCompile | - | compileSourceSetJava | 编译时给定sourceSet的依赖
sourceSetRuntime | - | - |运行时给定sourceSet的依赖

## 22.6.公共配置
Java插件会为project添加一些列的公共配置对象,如下所示,可以在构建脚本中使用这写属性,就像它们是该项目对象的属性(see [???](https://docs.gradle.org/current/userguide/java_plugin.html)).

**表22.7.Java插件-目录属性**

属性名称 | 类型 | 默认值 | 描述
 ----- | ---- | ---- | ----
 reportsDirName | String | reports | 在构建目录的生成报告的文件夹名
 reportsDir | File (read-only) | buildDir/reportsDirName | 该目录下会生成报告
 testResultsDirName | String | test-results | 在构建目录的测试结果的result.xml的存放目录名
 testResultsDir | File (read-only) | buildDir/testResultsDirName | 测试结果的 result.xml 文件会存放在该文件夹中
 testReportDirName | String |tests | 在构建目录的测试报告的文件夹名
 testReportDir | File (read-only) | reportsDir/testReportDirName | 测试的测试报告会存放在该目录下
 libsDirName | String | libs | 在构建目录下的类库文件夹名
 libsDir | File (read-only) | buildDir/libsDirName | 该目录下存放类库
 distsDirName | String | distributions | 在构建目录下的distributions文件夹名
 distsDir | File (read-only) | buildDir/distsDirName | 该目录下存放生成的distributions
 docsDirName | String | 在构建目录下的doc文件夹名
 docsDir | File (read-only) | buildDir/docsDirName | 该目录下存放生成的文档
 dependencyCacheDirName | String | dependency-cache | 在构建目录下的依赖缓存文件夹名
 dependencyCacheDir | File (read-only) | buildDir/dependencyCacheDirName | 该目录用来缓存源依赖信息。

 **表22.8.Java插件-其他配置**

 属性名称 | 类型 | 默认值 | 描述
  ----- | ---- | ---- | ----
  sourceSets | [SourceSetContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/SourceSetContainer.html) | Not null | 包含项目的sourceSet
  sourceCompatibility | [JavaVersion](https://docs.gradle.org/current/javadoc/org/gradle/api/JavaVersion.html).也可以使用String类型或Number类型,如'1.5' 或 1.5 | 当前使用的JVM版本 | 编译Java源码时所使用的Java兼容版本
  targetCompatibility | [JavaVersion](https://docs.gradle.org/current/javadoc/org/gradle/api/JavaVersion.html).也可以使用String类型或Number类型,如'1.5' 或 1.5 | sourceCompatibility | 生成class文件的Java版本
  archivesBaseName | String | projectName | 用于.jar文件或者.zip存档的基本名称
  manifest | [Mainfest](https://docs.gradle.org/current/javadoc/org/gradle/api/java/archives/Manifest.html) | an empty manifest | 该清单中包括所有的JAR文件

  按照[JavaPluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaPluginConvention.html)和[BasePluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.BasePluginConvention.html)类型提供这些属性.

## 22.7.使用sourceSet工作
可以使用sourceSet属性进行sourceSet,这是一个[SourceSetContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/SourceSetContainer.html)类型的项目的sourceSet容器.还可以通过sourceSet{}脚本块来配置sourceSet容器,sourceSet容器的工作方式基本与其他容器比如task的工作方式相同.

**例22.3.访问sourceSet**

**build.gradle**
```
//通过不同方式访问source set
println sourceSets.main.output.classesDir
println sourceSets['main'].output.classesDir
sourceSets{
  println main.output.classesDir
}
sourceSets{
  main{
    println output.classesDir
  }
}
// 遍历sourceSets
sourceSets.all{
  println name
}
```
配置现有的source set,只需要用上述的一种方式访问并设置source set,source set的配置如下所示,下面的例子主要配置了Java的main与resources目录:

**例22.4.配置一个source Set的资源路径**

**build.gradle**
```
sourceSets {
    main {
        java {
            srcDir 'src/java'
        }
        resources {
            srcDir 'src/resources'
        }
    }
}
```

### 22.7.1.source Set配置
下表列出了source Set的一些重要属性,更多细节请查看[SourceSet](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.SourceSet.html)的API文档.

**表22.9.java插件-source Set配置**

配置名称 | 类型 | 默认值 | 描述
--------- | ---------- | ---- | -----------
name | String (read-only) | Not null | 用来识别source set的名称
output | [SourceSetOutput](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.SourceSetOutput.html)(read-only) | Not null | source set的输出文件,包含其编译的classes和resources
output.classesDir | File | buildDir/classes/name | 在该目录下生成存放这个source set的classes文件
output.resourcesDir | File | buildDir/resources/name | 在该目录下生成存放这个source set的resources文件
compileClasspath | [FileCollection](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileCollection.html) | compileSourceSet configuration | 这个source set编译时使用的classpath
runtimeClasspath | [FileCollection](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileCollection.html) | output + runtimeSourceSet configuration | 执行当前source set的classes文件时的classpath
java | [SourceDirectorySet](https://docs.gradle.org/current/javadoc/org/gradle/api/file/SourceDirectorySet.html)(read-only) | Not null | 当前source set的java源文件,仅包含存在于java目录下的所有.java文件,排除其他任何文件.
java.srcDirs | Set<File>.可以设置为在[Section 15.5, “Specifying a set of input files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:specifying_multiple_files)中描述的任何值 | [projectDir/src/name/java] | 该source set的包含java源文件的目录
resources | [SourceDirectorySet](https://docs.gradle.org/current/javadoc/org/gradle/api/file/SourceDirectorySet.html)(read-only) | Not null | 该source set的资源,只包含存在于resource目录吓得资源文件,会排除在resource下的所有.java文件,其他插件,如Groovy插件会在该集合中排除一些其他的文件.
resources.srcDirs | Set<File>.可以设置为在[Section 15.5, “Specifying a set of input files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:specifying_multiple_files)中描述的任何值 | [projectDir/src/name/resources] | 该source set的包含资源文件的目录
allJava | [SourceDirectorySet](https://docs.gradle.org/current/javadoc/org/gradle/api/file/SourceDirectorySet.html)(read-only) | java | 该source set的所有.java文件。一些插件，如Groovy插件，添加额外的Java源文件到这个集合。
allSource | [SourceDirectorySet](https://docs.gradle.org/current/javadoc/org/gradle/api/file/SourceDirectorySet.html)(read-only) | resources + java	| 该source set的所有源文件。这包括所有的资源文件和所有Java源文件。一些插件，如Groovy插件，添加额外的源文件到这个集合。

### 22.7.2.定义一个新的source set
要定义一个新的源组，sourceSets {}块中引用它.下面是一个例子：

**例22.5.定义一个新的source set**

**build.gradle**
```
sourceSets {
    intTest
}
```
当你定义一个新的source set,java插件会为该source set添加一些如[Table 22.6, “Java plugin - source set dependency configurations”](https://docs.gradle.org/current/userguide/java_plugin.html#java_source_set_configurations)中所示的依赖配置关系.可以使用这些配置来定义source set的编译和运行时依赖。

**例22.6.定义source set的依赖**

**build.gradle**
```
sourceSets {
    intTest
}

dependencies {
    intTestCompile 'junit:junit:4.12'
    intTestRuntime 'org.ow2.asm:asm-all:4.0'
}
```
java插件增加了一些如[Table 22.2, “Java plugin - source set tasks”](https://docs.gradle.org/current/userguide/java_plugin.html#java_source_set_tasks)为该source set组装classes文件的任务,例如，对于一个叫intTest的source set，为此source set编译classes任务运行`gradle intTestClasses`完成。

**例22.7.编译一个source set**

`gradle intTestClasses`命令的输出
```
> gradle intTestClasses
:compileIntTestJava
:processIntTestResources
:intTestClasses

BUILD SUCCESSFUL

Total time: 1 secs
```
### 22.7.3.一些source set的例子
加入含有类文件的sorce set的JAR：

**例22.8.为source set组装JAR**

**build.gradle**
```
task intTestJar(type: Jar) {
    from sourceSets.intTest.output
}
```
为source set生成javadoc:

**例22.9.为source set生成javadoc**

**build.gradle**

```
task intTestJavadoc(type: Javadoc) {
    source sourceSets.intTest.allJava
}
```
为source set添加一个测试套件运行测试：

**例22.8.source set运行测试**

**build.gradle**
```
task intTest(type: Test) {
    testClassesDir = sourceSets.intTest.output.classesDir
    classpath = sourceSets.intTest.runtimeClasspath
}
```

## 22.8.Javadoc
java任务是一个[Javadoc](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.javadoc.Javadoc.html)的实例.它支持Javadoc的核心选项和可执行的Javadoc的[reference documentation](http://download.oracle.com/javase/1.5.0/docs/tooldocs/windows/javadoc.html#referenceguide)中描述的标准JavaTOC的选项。有关支持Javadoc选项的完整列表，请参阅以下类的API文档：[CoreJavadocOptions](https://docs.gradle.org/current/javadoc/org/gradle/external/javadoc/CoreJavadocOptions.html)和[StandardJavadocDocletOptions](https://docs.gradle.org/current/javadoc/org/gradle/external/javadoc/StandardJavadocDocletOptions.html).

**表22.10.java插件-javadoc配置**

任务属性 | 类型 | 默认值
--------- | ---------- | ----
classpath | [FileCollection](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileCollection.html) | sourceSets.main.output + sourceSets.main.compileClasspath
source | [FileTree](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileTree.html).可以设置为在[Section 15.5, “Specifying a set of input files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:specifying_multiple_files)中描述的任何值 | sourceSets.main.allJava
destinationDir | File | docsDir/javadoc
title | String | project的name和version

## 22.9.Clean
clean 任务是一个[Delete](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Delete.html)的实例.它只是表示删除其目录属性的目录。

**表22.11.java插件-清除配置**

任务属性 | 类型 | 默认值
--------- | ---------- | ----
dir | File | buildDir

## 22.10.资源
Java插件使用[Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html)任务处理资源.它为项目每个source set增加了一个实例.可以参考[Section 15.6, “Copying files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:copying_files)获取关于copy任务的信息.

**表22.12.java插件-ProcessResources配置**

任务属性 | 类型 | 默认值
--------- | ---------- | ----
srcDirs | Object.可以在[Section 15.5, “Specifying a set of input files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:specifying_multiple_files)中查看使用什么 | sourceSet.resources
destinationDir | File.可以再[Section 15.1, “Locating files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:locating_files)查看使用什么 | sourceSet.output.resourcesDir

## 22.11.编译java
java插件为项目的每一个source set增加了一个[JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.JavaCompile.html)实例,最常见的配置选项如下所示:

**表22.13.java插件-编译配置**

任务属性 | 类型 | 默认值
--------- | ---------- | ----
classpath | [FileCollection](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileCollection.html) | sourceSet.compileClasspath
source | [FileTree](https://docs.gradle.org/current/javadoc/org/gradle/api/file/FileTree.html),可以在[Section 15.6, “Copying files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:copying_files)中查看可以设置什么. | sourceSet.java
destinationDir | File. | sourceSet.output.classesDir

默认情况下java的编译运行在Gradle中的进程.设置选项
