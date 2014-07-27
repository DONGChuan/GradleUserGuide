# 快捷的任务定义

有一种比我们之前定义的 hello 任务更简明的方法

*Example 6.3. 快捷的任务定义

build.gradle*

    task hello << {
        println 'Hello world!'
    }

再一次, 它定义一个叫做 hello 的任务, 这个任务是一个可以执行的闭包.  我们将使用这种方式来定义这本指南里所有的任务.

