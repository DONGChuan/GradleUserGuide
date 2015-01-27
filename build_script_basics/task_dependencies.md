# 任务依赖

就像你所猜想的那样, 你可以申明任务之间的依赖关系.

*Example 6.6. 申明任务之间的依赖关系*

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

再加入一个依赖之前, 这个依赖的任务不需要提前定义了,来看下面的例子.

*Example 6.7. Lazy dependsOn - 其他的任务还没有存在*

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

taskX 到 taskY 的依赖在 taskY 被定义之前就已经申明了. 对于我们之后讲到的多任务构建是非常重要的. 任务依赖将会在 14.4 具体讨论.

请注意你不能使用快捷注释 (参考 5.8, “快捷注释”) 当所关联的任务还没有被定义.

