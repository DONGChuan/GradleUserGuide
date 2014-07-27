# 动态任务

Groovy 不仅仅被用来定义一个任务可以做什么. 举个例子, 你可以使用它来动态的创建任务.

*Example 6.8. 动态的创建一个任务*

*build.gradle*

    4.times { counter ->
        task "task$counter" << {
            println "I'm task number $counter"
        }
    }

**gradle -q task1** 命令的输出

    > gradle -q task1
    I'm task number 1
