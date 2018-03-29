# 执行 Tasks

你可以运行一个 task 和它所有的[依赖](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:task_dependencies).

```
❯ gradle myTask
```

You can learn about what projects and tasks are available in the [project reporting section](https://docs.gradle.org/current/userguide/command_line_interface.html#sec:command_line_project_reporting).

### Executing tasks in multi-project builds

In a[multi-project build](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html), subproject tasks can be executed with ":" separating subproject name and task name. The following are equivalent_when run from the root project_.

```
❯ gradle :mySubproject:taskName
❯ gradle mySubproject:taskName
```

You can also run a task for all subprojects using the task name only. For example, this will run the "test" task for all subprojects when invoked from the root project directory.

```
❯ gradle test
```

When invoking Gradle from within a subproject, the project name should be omitted:

```
❯ cd mySubproject
❯ gradle taskName
```

When executing the Gradle Wrapper from subprojects, one must reference`gradlew`relatively. For example:`../gradlew taskName`. The community[gdub project](http://www.gdub.rocks/)aims to make this more convenient.

### Executing multiple tasks

You can also specify multiple tasks. For example, the following will execute the`test`and`deploy`tasks in the order that they are listed on the command-line and will also execute the dependencies for each task.

```
❯ gradle test deploy
```

### Excluding tasks from execution

You can exclude a task from being executed using the`-x`or`--exclude-task`command-line option and providing the name of the task to exclude.

**Figure: Example Task Graph**

![](https://docs.gradle.org/current/userguide/img/commandLineTutorialTasks.png "Example Task Graph")

**Example: Excluding tasks**

Output of`gradle dist --exclude-task test`

```
>
 gradle dist --exclude-task test
:compile
compiling source
:dist
building the distribution

BUILD SUCCESSFUL in 0s
2 actionable tasks: 2 executed
```

You can see that the`test`task is not executed, even though it is a dependency of the`dist`task. The`test`task’s dependencies such as`compileTest`are not executed either. Those dependencies of`test`that are required by another task, such as`compile`, are still executed.

### Forcing tasks to execute

You can force Gradle to execute all tasks ignoring[up-to-date checks](https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:up_to_date_checks)using the`--rerun-tasks`option:

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

