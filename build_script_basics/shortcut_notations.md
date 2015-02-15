# 短标记法

正如同你已经在之前的示例里看到,
有一个短标记 $ 可以访问一个存在的任务. 也就是说每个任务都可以作为构建脚本的属性:

**例子 6.11. 当成构建脚本的属性来访问一个任务**

*build.gradle*

    task hello << {
        println 'Hello world!'
    }
    hello.doLast {
        println "Greetings from the $hello.name task."
    }

**gradle -q hello** 命令的输出

    > gradle -q hello
    Hello world!
    Greetings from the hello task.

这里的 name 是任务的默认属性,
代表当前任务的名称,
这里是 hello.

这使得代码易于读取，
特别是当使用了由插件（如编译）提供的任务时尤其如此.

