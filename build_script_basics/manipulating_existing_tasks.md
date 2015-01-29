# 使用已经存在的任务

当任务创建之后, 
它可以通过API来访问. 
这个和 Ant 不一样. 
举个例子, 
你可以创建额外的依赖.

#### 例子 5.9. 通过API访问一个任务 - 加入一个依赖

*build.gradle*

    4.times { counter ->
        task "task$counter" << {
            println "I'm task number $counter"
        }
    }
    task0.dependsOn task2, task3

**gradle -q task0** 命令的输出

    > gradle -q task0
    I'm task number 2
    I'm task number 3
    I'm task number 0

或者你可以给一个已经存在的任务加入行为.

#### 例子 5.10. 通过API访问一个任务 - 加入行为


*build.gradle*

    task hello << {
        println 'Hello Earth'
    }
    hello.doFirst {
        println 'Hello Venus'
    }
    hello.doLast {
        println 'Hello Mars'
    }
    hello << {
        println 'Hello Jupiter'
    }

**gradle -q hello** 命令的输出

    > gradle -q hello
    Hello Venus
    Hello Earth
    Hello Mars
    Hello Jupiter

doFirst 和 doLast 可以被执行许多次. 他们分别可以在任务动作列表的开始和结束加入动作. 
当任务执行的时候, 
在动作列表里的动作将被按顺序执行. 
这里第四个行为中 << 操作符是 doLast 的简单别称.

