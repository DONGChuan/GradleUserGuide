# 执行 Tasks

你可以运行一个 task 和它所有的[依赖](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:task_dependencies).

```
❯ gradle myTask
```

### 在多项目构建中执行 tasks

在一个 [多项目构建当中](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html), 子项目的 tasks 可以通过在子项目名后添加 ":" 和 task 名来执行:

```
❯ gradle :mySubproject:taskName
❯ gradle mySubproject:taskName
```

> 当从根项目执行时, 上面2个是相等的

当只使用 task 名时, 这个任务是对所有子项目都执行的. 举个例子, 在项目根目录运行下面的命令将对所有子项目都执行 "test" task.

```
❯ gradle test
```

当在一个子项目中执行命令, 就不需要显式的指定子项目名称, 且该任务只对子项目本身执行:

```
❯ cd mySubproject
❯ gradle taskName
```

当在子项目中执行 Gradle Wrapper, 必须引用`gradlew`的相对路径. 举个例子:`../gradlew taskName`. 社区 [gdub project](http://www.gdub.rocks/) 可以提供更多的支持.

### 执行多个 tasks

你也可以同时指定执行多个任务. 举个例子, 下面的命令将按命令行中的顺序执行`test`和`deploy`命令, 当然, 也会执行各个 task 的依赖:

```
❯ gradle test deploy
```

### 从执行中排除 tasks

你可以通过使用`-x`或`--exclude-task`加上任务名排除某个 task 的执行:

**Figure: Example Task Graph**

![](https://docs.gradle.org/current/userguide/img/commandLineTutorialTasks.png "Example Task Graph")

**Example: Excluding tasks**

`gradle dist --exclude-task test`的输出:

```
>gradle dist --exclude-task test
:compile
compiling source
:dist
building the distribution

BUILD SUCCESSFUL in 0s
2 actionable tasks: 2 executed
```

你可以看到`test`任务没有被执行, 即使它是`dist`任务的依赖. `test`任务的依赖比如`compileTest`也没有被执行. 但是其他依赖, 比如`compile`, 它仍被其它的任务所依赖, 所以仍然会被执行.

### 强制执行任务

你可以强制 Gradle to execute all tasks ignoring[up-to-date checks](https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:up_to_date_checks)using the`--rerun-tasks`option:

```
❯ gradle test --rerun-tasks
```

This will force`test`and\_all\_task dependencies of`test`to execute. It’s a little like running`gradle clean test`, but without the build’s generated output being deleted.

### Continuing the build when a failure occurs

By default, Gradle will abort execution and fail the build as soon as any task fails. This allows the build to complete sooner, but hides other failures that would have occurred. In order to discover as many failures as possible in a single build execution, you can use the`--continue`option.

```
❯ gradle test --continue
```

When executed with`--continue`, Gradle will execute\_every\_task to be executed where all of the dependencies for that task completed without failure, instead of stopping as soon as the first failure is encountered. Each of the encountered failures will be reported at the end of the build.

If a task fails, any subsequent tasks that were depending on it will not be executed. For example, tests will not run if there is a compilation failure in the code under test; because the test task will depend on the compilation task \(either directly or indirectly\).

### Task name abbreviation

When you specify tasks on the command-line, you don’t have to provide the full name of the task. You only need to provide enough of the task name to uniquely identify the task. For example, it’s likely`gradle che`is enough for Gradle to identify the`check`task.

You can also abbreviate each word in a camel case task name. For example, you can execute task`compileTest`by running`gradle compTest`or even`gradle cT`.

**Example: Abbreviated camel case task name**

Output of`gradle cT`

```
>
 gradle cT
:compile
compiling source
:compileTest
compiling unit tests

BUILD SUCCESSFUL in 0s
2 actionable tasks: 2 executed
```

You can also use these abbreviations with the -x command-line option.

