# 排除任务

你可以用命令行选项-x来排除某些任务,让我们用上面的例子来示范一下.

*例子 10.2. 排除任务*

**gradle dist -x test**  命令的输出

    > gradle dist -x test
    :compile
    compiling source
    :dist
    building the distribution

    BUILD SUCCESSFUL

    Total time: 1 secs

可以看到, test 任务并没有被调用,即使它是 dist 任务的依赖. 同时 test 任务的依赖任务 compileTest 也没有被调用,而像 compile 被 test 和其它任务同时依赖的任务仍然会被调用.


