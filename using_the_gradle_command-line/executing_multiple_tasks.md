# 多任务调用

你可以以列表的形式在命令行中一次调用多个任务.
例如 **gradle compile test** 命令会依次调用 compile 和 test 任务, 它们所依赖的任务也会被调用. 这些任务只会被调用一次, 无论它们是否被包含在脚本中:即无论是以命令行的形式定义的任务还是依赖于其它任务都会被调用执行.来看下面的例子.

下面定义了四个任务 dist和test 都 依赖于 compile ,只用当 compile 被调用之后才会调用 gradle dist test 任务


示例图 10.1. 任务依赖

![Task dependencies](http://pkaq.github.io/gradledoc/docs/userguide/img/commandLineTutorialTasks.png)

*例子 10.1. 多任务调用*

*build.gradle*

    task compile << {
        println 'compiling source'
    }

    task compileTest(dependsOn: compile) << {
        println 'compiling unit tests'
    }

    task test(dependsOn: [compile, compileTest]) << {
        println 'running unit tests'
    }

    task dist(dependsOn: [compile, test]) << {
        println 'building the distribution'
    }

**gradle dist test** 命令的输出

    > gradle dist test
    :compile
    compiling source
    :compileTest
    compiling unit tests
    :test
    running unit tests
    :dist
    building the distribution

    BUILD SUCCESSFUL

    Total time: 1 secs

由于每个任务仅会被调用一次,所以调用**gradle test test**与调用**gradle test**效果是相同的.

