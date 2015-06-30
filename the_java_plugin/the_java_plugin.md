# 22章 Java插件
Java插件给一个项目增加了编译，测试以及绑定Java的能力，许多 Gradle 的插件都需要以 Java 插件为基础
## 22.1.用法
要使用 Java 脚本，在构建脚本中加入如下内容

**例子 22.1.使用 Java 插件**

**bulid.gradle**

```
apply plugin: 'java'
```

## 22.2.资源设置
Java 插件引入了*_资源设置(Source Set)_*的概念，资源设置就是一组编译和执行的文件。
这些源文件可能包含 Java 的源文件以及一些资源文件。
其他的插件可能还会在资源设置中包含Groovy 和Scala的源文件,资源设置有一个与之关联的编译classpath和运行classpath。

使用资源设置的目的是为了将源文件根据它们的目的去归档到不同的逻辑组，举个例子，你可以使用一个资源设置来定义一个集成测试套件
也可以使用另外的源组来定义你项目的 API 和实现类。

Java 插件定义了两个标准资源设置，分别为`main`和`test`,`main`资源设置中包含了生产代码并且会编译并组装成 Jar 文件，
`test`资源设置包括了测试代码并且会使用JUnit或者TestNG编译和执行。它们可以是单元测试，集成测试，验收测试或者任何对你有用的测试集。

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

对于添加到项目中的每个每个资源设置，java 插件加入了以下编译任务

**表22.2.java 插件-资源设置任务**

任务名 | 依赖 | 类型 | 描述
--------- | ---------- | ---- | -----------
compile*SourceSet*Java | 产生资源设置编译 classpath 的所有任务 | [JavaCompile](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.Configuration.html) | 使用 javac 命令编译给定资源设置的 Java 源文件
processSourceSetResources | - | [Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html) | 复制给定资源设置的资源到classes目录下。
sourceSetClasses | compileSourceSetJava任务和processSourceSetResources任务。一些插件给资源设置添加额外的编译工作。 | [Task](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html) | 组装资源设置的class目录

Java 插件和增加了一些为项目构建生命周期的任务

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

## 22.4.项目布局
Java 插件的默认布局如下图所示，无论这些文件夹中有没有内容，Java插件都会编译里面的内容，并处理缺失的内容。

**表22.4.java 插件-默认布局**

目录 | 含义
 --— | ---
src/main/java |  主要 Java 源码
src/main/resources | 主要资源
src/test/java | 测试 Java 源码
src/test/resources | 测试资源
src/sourceSet/java | 指定资源设置的 Java 源码
src/sourceSet/resources | 指定资源设置的资源

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

名称 | 扩展 | 被使用时运行的任务 | 含义
--------- | ---------- | ---- | -----------
compile | - | compileJava | 编译时的依赖
runtime | compile | - | 运行时的依赖
testCompile | compile | compileTestJava | 编译测试所需的额外依赖
testRuntime | runtime | test | 仅供运行测试的额外依赖
archives | - | uploadArchives | 项目产生的信息单元（如:jar包）
default | runtime | - | 使用其他项目的默认依赖项，包括该项目产生的信息单元以及依赖

**图22.2.Java插件-依赖配置**
![java plugin-dependency configurations](https://docs.gradle.org/current/userguide/img/javaPluginConfigurations.png)

对于每个添加到该项目的资源设置，java插件会添加一下的依赖配置

**表22.6.Java插件-资源设置依赖关系配置**

名称 | 扩展 | 被使用时运行的任务 | 含义
--------- | ---------- | ---- | -----------
sourceSetCompile | - | compileSourceSetJava | 编译时给定资源设置的依赖
sourceSetRuntime | - | - |运行时给定资源设置的依赖

## 22.6.公共配置
Java插件会为project添加一些列的公共配置,如下所示,可以在构建脚本中使用这写属性,就像它们是该项目对象的属性(see [???](https://docs.gradle.org/current/userguide/java_plugin.html)).

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
  sourceSets | [SourceSetContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/SourceSetContainer.html) | Not null | 包含项目的资源设置
  sourceCompatibility | [JavaVersion](https://docs.gradle.org/current/javadoc/org/gradle/api/JavaVersion.html).也可以使用String类型或Number类型,如'1.5' 或 1.5 | 当前使用的JVM版本 | 编译Java源码时所使用的Java兼容版本
  targetCompatibility | [JavaVersion](https://docs.gradle.org/current/javadoc/org/gradle/api/JavaVersion.html).也可以使用String类型或Number类型,如'1.5' 或 1.5 | sourceCompatibility | 生成class文件的Java版本
  archivesBaseName | String | projectName | 用于.jar文件或者.zip存档的基本名称
  manifest | [Mainfest](https://docs.gradle.org/current/javadoc/org/gradle/api/java/archives/Manifest.html) | an empty manifest | 该清单中包括所有的JAR文件

  按照[JavaPluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaPluginConvention.html)和[BasePluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.BasePluginConvention.html)类型提供这些属性.

## 22.7.使用资源设置
