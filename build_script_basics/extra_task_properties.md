# 附加的 task 属性

你可以给任务加入你自己的属性. 为了加入一个 myProperty 属性, 设置一个初始值给 ext.myProperty. 从这一点上来说，该属性可以读取和设置像一个预定义的任务属性.

*Example 6.12. 给任务加入额外的属性*

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

给任务加额外的属性是没有限制的. 你可以在 13.4.2, “额外属性” 里获得更多的信息.

