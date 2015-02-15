# 使用方法

Gradle 能很好地衡量你编写脚本的逻辑能力. 首先要做的是如何提取一个方法.

**例子 6.14. 使用方法组织脚本逻辑**

*build.gradle*

    task checksum << {
        fileList('../antLoadfileResources').each {File file ->
            ant.checksum(file: file, property: "cs_$file.name")
            println "$file.name Checksum: ${ant.properties["cs_$file.name"]}"
        }
    }

    task loadfile << {
        fileList('../antLoadfileResources').each {File file ->
            ant.loadfile(srcFile: file, property: file.name)
            println "I'm fond of $file.name"
        }
    }

    File[] fileList(String dir) {
        file(dir).listFiles({file -> file.isFile() } as FileFilter).sort()
    }

**adle -q loadfile** 命令的输出

    > gradle -q loadfile
    I'm fond of agile.manifesto.txt
    I'm fond of gradle.manifesto.txt


稍后你看到,
这种方法可以在多项目构建的子项目之间共享. 如果你的构建逻辑变得更加复杂,
Gradle 为你提供了其他非常方便的方法. 请参见第59章，组织构建逻辑。

