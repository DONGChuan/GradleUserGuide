# 项目报告

Gradle 本身含有一些内建任务来显示构建的一些特别的详细信息. 可以帮助用户来更好的理解构建的结构和依赖, 来更好的解决问题.

你可以直接使用 `gradle help` 来获得相关的帮助信息.

### 列出所有项目

运行 `gradle projects` 来显示所选项目的所有子项目以及它们的层级关系.

```
❯ gradle projects
```

你也可以通过 build scans 来获得完整的项目报告. 阅读[创建 build scans](https://guides.gradle.org/creating-build-scans/)了解更多.

### 列出所有任务

运行 `gradle tasks` 来显示所选项目的主要任务以及各个任务的描述.

```
❯ gradle tasks
```

默认的, 这个报告只显示已经被分组的任务. 你可以通过使用 `--all` 选项来获得更多任务信息.

```
❯ gradle tasks --all
```

### 显示任务的使用详情

使用 `gradle help --task someTask` 命令可以显示指定任务的详细信息.

**Example: 获得任务的详细信息**

**`gradle -q help --task libs`** 的输出:

```
> gradle -q help --task libs
Detailed task information for libs

Paths
     :api:libs
     :webapp:libs

Type
     Task (org.gradle.api.Task)

Description
     Builds the JAR

Group
     build

```

通过上面的输出, 你可以看到完整的任务路径, 类型, 可能的命令行选项以及任务的描述和分组.

### 显示依赖的详情

Build scans 可以显示完整的依赖详情, 比如哪些配置有哪些依赖, 依赖的版本等等.

```
❯ gradle myTask --scan
```

使用这个命令可以生成一个链接, 指向一个网页报告, 你可以发现如下的依赖信息:

![](https://docs.gradle.org/current/userguide/img/gradle-core-test-build-scan-dependencies.png "Build Scan dependencies report")

阅读[检查依赖](https://docs.gradle.org/current/userguide/inspecting_dependencies.html)了解更多.

### 显示项目的所有依赖

运行 `gradle dependencies` 命令将列出指定项目的所有依赖, 以每一个配置作为分隔. 对于每一个配置, 它的直接的依赖或者 transitive dependencies 都将以树形结构来显示:

```
❯ gradle dependencies
```

具体的例子可以阅读[检测依赖](https://docs.gradle.org/current/userguide/inspecting_dependencies.html)了解更多.

运行 `gradle buildEnvironment` 可以显示所选项目的构建脚本的依赖, 和 `gradle dependencies` 一样, 显示正在构建的软件的依赖.

```
❯ gradle buildEnvironment
```

运行 `gradle dependencyInsight` 可以了解一个或几个匹配特别输入的特别的依赖.

```
❯ gradle dependencyInsight
```

有时候, 依赖报告会非常大, 你可以添加 `--configuration` 选项来限制只报告某个特别的配置:

### 显示项目的所有属性

运行 `gradle properties` 显示所选项目的所有属性.

**Example: 属性的信息**

**`gradle -q api:properties`**的输出:

```
> gradle -q api:properties

------------------------------------------------------------
Project :api - The shared API for the application
------------------------------------------------------------

allprojects: [project ':api']
ant: org.gradle.api.internal.project.DefaultAntBuilder@12345
antBuilderFactory: org.gradle.api.internal.project.DefaultAntBuilderFactory@12345
artifacts: org.gradle.api.internal.artifacts.dsl.DefaultArtifactHandler_Decorated@12345
asDynamicObject: DynamicObject for project ':api'
baseClassLoaderScope: org.gradle.api.internal.initialization.DefaultClassLoaderScope@12345
buildDir: /home/user/gradle/samples/userguide/tutorial/projectReports/api/build
buildFile: /home/user/gradle/samples/userguide/tutorial/projectReports/api/build.gradle

```

### 软件模型的相关报告

通过 `model` 任务, 你可以的得到一个[软件模型](https://docs.gradle.org/current/userguide/software_model.html) 项目的各个元素的层级显示:

```
❯ gradle model
```

阅读[模型报告](https://docs.gradle.org/current/userguide/software_model.html#model-report)了解更多.

