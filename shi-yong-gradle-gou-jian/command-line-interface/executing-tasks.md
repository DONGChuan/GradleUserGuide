# 执行 Tasks

你可以运行一个 task 和它所有的[依赖](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html#sec:task_dependencies).

```
> gradle myTask
```

### 在多项目构建中执行 tasks

在一个 [多项目构建当中](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html), 子项目的 tasks 可以通过在子项目名后添加 ":" 和 task 名来执行:

```
> gradle :mySubproject:taskName
> gradle mySubproject:taskName
```

> 当从根项目执行时, 上面2个是相等的

当只使用 task 名时, 这个任务是对所有子项目都执行的. 举个例子, 在项目根目录运行下面的命令将对所有子项目都执行 "test" task.

```
❯ gradle test
```

当在一个子项目中执行命令, 就不需要显式的指定子项目名称, 且该任务只对子项目本身执行:

```
> cd mySubproject
> gradle taskName
```

当在子项目中执行 Gradle Wrapper, 必须引用`gradlew`的相对路径. 举个例子:`../gradlew taskName`. 社区 [gdub project](http://www.gdub.rocks/) 可以提供更多的支持.

### 执行多个 tasks

你也可以同时指定执行多个任务. 举个例子, 下面的命令将按命令行中的顺序执行`test`和`deploy`命令, 当然, 也会执行各个 task 的依赖:

```
> gradle test deploy
```

### 从执行中排除 tasks

你可以通过使用`-x`或`--exclude-task`加上任务名排除某个 task 的执行:

**Figure: Example Task Graph**

![](https://docs.gradle.org/current/userguide/img/commandLineTutorialTasks.png "Example Task Graph")

**Example: 排除任务**

`gradle dist --exclude-task test`的输出:

```
> gradle dist --exclude-task test
:compile
compiling source
:dist
building the distribution

BUILD SUCCESSFUL in 0s
2 actionable tasks: 2 executed
```

你可以看到`test`任务没有被执行, 即使它是`dist`任务的依赖. `test`任务的依赖比如`compileTest`也没有被执行. 但是其他依赖, 比如`compile`, 它仍被其它的任务所依赖, 所以仍然会被执行.

### 强制执行任务

许多任务, 特别是 Gradle 本身提供的任务都支持增量版本. 这些任务可以根据它们的输入或输出自上次运行以来是否发生变化来确定它们是否需要运行. 当 Gradle 在构建运行期间在其名称旁边显示文本`UP-TO-DATE`时，你就可以轻松识别参与增量构建的任务. 但是有时你可能偶尔想强制 Gradle 运行所有的任务, 不需要判断是否需要运行. 如果是这样, 只要使用`--rerun-tasks`选项即可:

```
> gradle test --rerun-tasks
```

该命令就会强制执行`test`以及它所有的依赖.

### 发生错误时继续执行构建

默认, 只要任务失败, Gradle 就会放弃执行构建. 随后其他的任务都不会执行了, 但是这样会漏掉这些还未执行的任务里的错误, 你可以使用 `--continue` 选项来解决这个问题.

```
> gradle test --continue
```

当执行 `--continue` 时, Gradle 将执行每一个任务, 前提是这每一个任务的依赖都不出错. 这样所有可以执行的任务都将执行, 而不是出现第一个错误就停止. 每一个监听到的错误都会在构建结束后打印出来.

如果一个任务失败, 所有随后依赖于它的任务都不会执行. 比如, 如果有个编译错误, 那随后的任务就不会执行. 因为测试任务是依赖于编译任务的(直接或间接).

### 任务名缩写

当你在命令行里写任务名称时, 不需要写任务的全名. 只需要输入能唯一识别该任务的名称即可. 比如, `gradle che` 可以缩写为 `check` 任务.

你也可以使用驼峰形式的缩写来识别任务. 比如任务 `compileTest` 可以缩写为 `gradle compTest` 或者 `gradle cT`.

**Example: 驼峰式缩写**

`gradle cT` 的输出:

```
> gradle cT
:compile
compiling source
:compileTest
compiling unit tests

BUILD SUCCESSFUL in 0s
2 actionable tasks: 2 executed
```


当使用之前讲的 -x 命令行来排除某个任务执行的时候也可以使用任务名缩写.