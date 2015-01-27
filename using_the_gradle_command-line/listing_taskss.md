# 任务列表

执行 **gradle tasks** 命令会列出项目中所有任务.
这会显示项目中所有的默认任务以及每个任务的描述.

例 10.9 获取任务信息

**gradle -q tasks** 命令的输出

```
> gradle -q tasks
------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Default tasks: dists

Build tasks
-----------
clean - Deletes the build directory (build)
dists - Builds the distribution
libs - Builds the JAR

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'projectReports'.
dependencyInsight - Displays the insight into a specific dependency in root project 'projectReports'.
help - Displays a help message
projects - Displays the sub-projects of root project 'projectReports'.
properties - Displays the properties of root project 'projectReports'.
tasks - Displays the tasks runnable from root project 'projectReports' (some of the displayed tasks may belong to subprojects).

To see all tasks and more detail, run with --all.```

默认情况下,这只会显示那些被分组的任务.
你可以通过为任务设置 **group** 属性和 **description** 来把 这些信息展示到结果中.

例 10.10. 更改任务报告内容

*build.gradle*

```
dists {
    description = 'Builds the distribution'
    group = 'build'
}```

当然你也可以用 **--all** 参数来收集更多任务信息.
这会列出项目中所有任务以及任务之间的依赖关系.

例 10.11 获得更多的任务信息

**gradle -q tasks --all** 命令的输出

```
> gradle -q tasks --all
------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Default tasks: dists

Build tasks
-----------
clean - Deletes the build directory (build)
api:clean - Deletes the build directory (build)
webapp:clean - Deletes the build directory (build)
dists - Builds the distribution [api:libs, webapp:libs]
    docs - Builds the documentation
api:libs - Builds the JAR
    api:compile - Compiles the source files
webapp:libs - Builds the JAR [api:libs]
    webapp:compile - Compiles the source files

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Help tasks
----------
dependencies - Displays all dependencies declared in root project 'projectReports'.
api:dependencies - Displays all dependencies declared in project ':api'.
webapp:dependencies - Displays all dependencies declared in project ':webapp'.
dependencyInsight - Displays the insight into a specific dependency in root project 'projectReports'.
api:dependencyInsight - Displays the insight into a specific dependency in project ':api'.
webapp:dependencyInsight - Displays the insight into a specific dependency in project ':webapp'.
help - Displays a help message
api:help - Displays a help message
webapp:help - Displays a help message
projects - Displays the sub-projects of root project 'projectReports'.
api:projects - Displays the sub-projects of project ':api'.
webapp:projects - Displays the sub-projects of project ':webapp'.
properties - Displays the properties of root project 'projectReports'.
api:properties - Displays the properties of project ':api'.
webapp:properties - Displays the properties of project ':webapp'.
tasks - Displays the tasks runnable from root project 'projectReports' (some of the displayed tasks may belong to subprojects).
api:tasks - Displays the tasks runnable from project ':api'.
webapp:tasks - Displays the tasks runnable from project ':webapp'.```

