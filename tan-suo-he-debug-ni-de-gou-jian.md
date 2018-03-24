# 探索和DEBUG你的构建

让我们来看看我们还能在新的项目里做些什么. 可以查看[reference to the command-line interface](https://docs.gradle.org/4.6/userguide/command_line_interface.html)获得完整的命令行指引.

### 查看可以使用的任务

`tasks`命令会列出我们当前可以调用的 Gradle 任务,包括通过`base`插件添加的任务以及我们刚刚添加的任务.

```
❯ ./gradlew tasks

> Task :tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Archive tasks
-------------
zip - Archives sources in a zip file

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
clean - Deletes the build directory.

Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.

Custom tasks
------------
copy - Simply copies sources to a the build directory

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'basic-demo'.
components - Displays the components produced by root project 'basic-demo'. [incubating]
dependencies - Displays all dependencies declared in root project 'basic-demo'.
dependencyInsight - Displays the insight into a specific dependency in root project 'basic-demo'.
dependentComponents - Displays the dependent components of components in root project 'basic-demo'. [incubating]
help - Displays a help message.
model - Displays the configuration model of root project 'basic-demo'. [incubating]
projects - Displays the sub-projects of root project 'basic-demo'.
properties - Displays the properties of root project 'basic-demo'.
tasks - Displays the tasks runnable from root project 'basic-demo'.

Verification tasks
------------------
check - Runs all checks.

Rules
-----
Pattern: clean<TaskName>: Cleans the output files of a task.
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.

To see all tasks and more detail, run gradlew tasks --all

To see more detail about a task, run gradlew help --task <task>

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```

### 分析和DEBUG你的构建 {#analyze_and_debug_your_build}

Gradle 提供了一个基于网页的分析工具 [build scan](https://scans.gradle.com/)

.![](/assets/basic-demo-build-scan.png)可以通过`--scan`选项或者显式的在项目中添加 build scan 插件, 你就可以免费的在 link:scans.gradle.com 上创建一个 buildscan. 将 build scans 发布到 scans.gradle.com 将传输[该项数据](https://docs.gradle.com/scans/#captured_information) 到 Gradle 的服务器. 如果要在你自己的服务器中保管数据, 请查阅 [Gradle Enterprise](https://gradle.com/enterprise).

下面来尝试在执行任务的时候添加 `--scan` 选项:

```
❯ ./gradlew zip --scan

BUILD SUCCESSFUL in 0s
1 actionable task: 1 up-to-date

Publishing a build scan to scans.gradle.com requires accepting the Terms of Service defined 
at https://scans.gradle.com/terms-of-service. Do you accept these terms? [yes, no]
Gradle Cloud Services license agreement accepted.

Publishing build scan...
https://gradle.com/s/repnge6srr5qs

```



如果你仔细浏览你的 build scan, 你应该可以直观的发现任务执行的位置以及时长, 以及其他信息. 如果需要学习更多的 build scans 的用法, 请查阅 [Build Scan Plugin User Manual](https://docs.gradle.com/build-scan-plugin/).



