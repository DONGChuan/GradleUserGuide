# 调用 Ant 任务

Ant 任务是 Gradle 的一等公民.
Gradle 通过 Groovy 出色的集成了 Ant 任务.
Groovy 自带了一个 AntBuilder.
相比于从一个 build.xml 文件中使用 Ant 任务,
在 Gradle 里使用 Ant 任务更为方便和强大. 从下面的例子中,
你可以学习如何执行 Ant 任务以及如何访问 ant 属性：

**例子 6.13. 使用 AntBuilder 来执行 ant.loadfile 任务**

*build.gradle*

    task loadfile << {
        def files = file('../antLoadfileResources').listFiles().sort()
        files.each { File file ->
            if (file.isFile()) {
                ant.loadfile(srcFile: file, property: file.name)
                println " *** $file.name ***"
                println "${ant.properties[file.name]}"
            }
        }
    }

**gradle -q loadfile** 命令的输出

    > gradle -q loadfile
    *** agile.manifesto.txt ***
    Individuals and interactions over processes and tools
    Working software over comprehensive documentation
    Customer collaboration  over contract negotiation
    Responding to change over following a plan
     *** gradle.manifesto.txt ***

