# 使用 Ant 任务

Ant 任务是 Gradle 的一等公民. Gradle 通过 Groovy 出色的集成了 Ant 任务. Groovy 和 AntBuilder 装在在一起. 相比于使用Ant任务从一个build.xml文件, 在 Gradle 里使用 Ant 任务是为方便和强大. 从下面的例子中，你可以学习如何执行 Ant 任务以及如何访问 ant 属性：

*Example 6.13. 使用 AntBuilder 来执行 ant.loadfile 任务*

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

使不可能成为可能, 使可能更加简单，使简单更加优雅.
