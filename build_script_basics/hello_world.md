# Hello world

你可以通过 **gradle** 命令运行一个 Gradle 构建.

**gradle** 命令会在当前目录中查找一个叫 **build.gradle** 的文件.
我们称这个 build.gradle 文件为一个构建脚本 (build script), 但是严格来说它是一个构建配置脚本 (build configuration script). 这个脚本定义了一个 project 和它的 tasks.

让我们来先看一个例子,
创建一个名为build.gradle的构建脚本.

**例子 6.1 第一个构建脚本**

*build.gradle*

    task hello {
        doLast {
            println 'Hello world!'
        }
    }

在命令行里, 进入脚本所在的文件夹然后输入 **gradle -q hello** 来执行构建脚本:

**gradle -q hello** 的输出
    > gradle -q hello
    Hello world!

这里发生了什么? 这个构建脚本定义了一个独立的 task,
叫做 hello,
并且加入了一个 action.
当你运行 **gradle hello**,
Gradle 执行叫做 hello 的 task,
也就是执行了你所提供的 action.
这个 action 是一个包含了一些 Groovy 代码的闭包(closure 这个概念不清楚的同学好好谷歌下).

如果你认为这些看上去和 Ant 的 targets 很相像,
好吧,
你是对的.
Gradle tasks 和 Ant 的 targets 是对等的.
但是你将会会看到,
Gradle tasks 更加强大.
我们使用一个不同于 Ant 的术语 task,
看上去比 target 更加能直白.
不幸的是这带来了一个术语冲突,
因为 Ant 称它的命令,
比如 javac 或者 copy,
叫 tasks.
所以当我们谈论 tasks,
是指 Gradle 的 tasks.
如果我们讨论 Ant 的 tasks (Ant 命令), 我们会直接称呼 ant task.

####补充一点命令里的 **-q** 是干什么的?

这个指南里绝大多说的例子会在命令里加入 **-q**.
代表 quiet 模式.
它不会生成 Gradle 的日志信息 (log messages),
所以用户只能看到 tasks 的输出.
它使得的输出更加清晰.
你并不一定需要加入这个选项.
参考第 18 章,
日志的 Gradle 影响输出的详细信息.



