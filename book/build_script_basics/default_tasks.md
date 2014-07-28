# 默认任务

Gradle 允许你定义一个或多个默认任务.

*Example 6.15. 定义一个默认任务*

*build.gradle*

    defaultTasks 'clean', 'run'

    task clean << {
        println 'Default Cleaning!'
    }

    task run << {
        println 'Default Running!'
    }

    task other << {
        println "I'm not a default task!"
    }

**gradle -q** 命令的输出

    > gradle -q
    Default Cleaning!
    Default Running!

等价于 **gradle clean run**. 在一个多项目构建中, 每一个子项目都可以有它特别的默认任务. 如果一个子项目没有特别的默认任务, 父项目的默认任务将会被执行.


