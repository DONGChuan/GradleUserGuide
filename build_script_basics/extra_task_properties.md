# 自定义任务属性

你可以给任务加入自定义的属性.
列如加入一个叫做 myProperty 属性,
设置一个初始值给 ext.myProperty.
然后,
该属性就可以像一个预定义的任务属性那样被读取和设置了.

**例子 6.12. 给任务加入自定义属性**

*build.gradle*

    task myTask {
        ext.myProperty = "myValue"
    }

    task printTaskProperties << {
        println myTask.myProperty
    }

**gradle -q printTaskProperties** 命令的输出

    > gradle -q printTaskProperties
    myValue

给任务加自定义属性是没有限制的. 你可以在 13.4.2, “自定义属性” 里获得更多的信息.

