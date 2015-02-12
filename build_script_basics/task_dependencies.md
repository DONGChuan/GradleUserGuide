# 任务依赖

就像你所猜想的那样,
你可以声明任务之间的依赖关系.

**例子 6.6. 申明任务之间的依赖关系**

*build.gradle*

    task hello << {
        println 'Hello world!'
    }

    task intro(dependsOn: hello) << {
        println "I'm Gradle"
    }

**gradle -q intro** 命令的输出

    > gradle -q intro
    Hello world!
    I'm Gradle

intro 依赖于 hello,
所以执行 intro 的时候 hello 命令会被优先执行来作为启动 intro 任务的条件.

在加入一个依赖之前,
这个依赖的任务不需要提前定义,
来看下面的例子.

**例子 6.7. Lazy dependsOn - 其他的任务还没有存在**

*build.gradle*

    task taskX(dependsOn: 'taskY') << {
        println 'taskX'
    }
    task taskY << {
        println 'taskY'
    }

**gradle -q taskX** 命令的输出

    > gradle -q taskX
    taskY
    taskX

taskX 到 taskY 的依赖在 taskY 被定义之前就已经声明了. 这一点对于我们之后讲到的多任务构建是非常重要的.
任务依赖将会在 14.4 具体讨论.

请注意你不能使用快捷注释 (参考 6.8, “快捷注释”) 当所关联的任务还没有被定义.

