# 定制项目

Java 插件给项目加入了一些属性 (propertiy) . 这些属性已经被定义了默认的值, 已经足够来开始构建项目了. 如果你认为不合适, 改变它们的值也是很简单的. 让我们看下这个例子. 这里我们将指定 Java 项目的版本号, 以及我们所使用的 Java 的版本. 我们同样也加入了一些属性在 jar 的清单里.

*例子 6.5. 定制 MANIFEST.MF 文件*

*build.gradle*

    sourceCompatibility = 1.5
    version = '1.0'
    jar {
        manifest {
            attributes 'Implementation-Title': 'Gradle Quickstart', 'Implementation-Version': version
        }
    }

Java 插件加入的任务是常规性的任务, 准确地说, 就如同它们在构建文件里声明地一样. 这意味着你可以使用任务之前的章节提到的方法来定制这些任务T. 举个例子, 你可以设置一个任务的属性, 在任务里加入行为, 改变任务的依赖, 或者完全重写一个任务, 我们将配置一个测试任务, 当测试执行的时候它会加入一个系统属性:

*例子 6.6. 测试阶段加入一个系统属性*

*build.gradle*

    test {
        systemProperties 'property': 'value'
    }

哪些属性是可用的?

你可以使用 **gradle properties** 命令来列出项目的所有属性. 这样你就可以看到 Java 插件加入的属性以及它们的默认值.
