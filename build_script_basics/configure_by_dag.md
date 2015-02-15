# 通过 DAG 配置

正如我们之后的详细描述 (参见第55章，构建的生命周期), Gradle 有一个配置阶段和执行阶段.
在配置阶段后,
Gradle 将会知道应执行的所有任务.
Gradle 为你提供一个"钩子",
以便利用这些信息.
举个例子,
判断发布的任务是否在要被执行的任务当中.
根据这一点,
你可以给一些变量指定不同的值.

在接下来的例子中,
distribution 任务和 release 任务将根据变量的版本产生不同的值.

**例子 6.16. 根据选择的任务产生不同的输出**

*build.gradle*

    task distribution << {
        println "We build the zip with version=$version"
    }

    task release(dependsOn: 'distribution') << {
        println 'We release now'
    }

    gradle.taskGraph.whenReady {taskGraph ->
        if (taskGraph.hasTask(release)) {
            version = '1.0'
        } else {
            version = '1.0-SNAPSHOT'
        }
    }

**gradle -q distribution** 命令的输出

    > gradle -q distribution
    We build the zip with version=1.0-SNAPSHOT
    Output of gradle -q release
    > gradle -q release
    We build the zip with version=1.0
    We release now

最重要的是 whenReady 在 release 任务执行之前就已经影响了 release 任务.
甚至 release 任务不是首要任务 (i.e., 首要任务是指通过 gradle 命令的任务).


