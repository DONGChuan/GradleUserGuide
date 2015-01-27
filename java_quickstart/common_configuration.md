# 通用配置

对于绝大多数多项目构建, 有一些配置对所有项目都是常见的或者说是通用的. 在我们的例子里, 我们将在根项目里定义一个这样的通用配置, 使用一种叫做配置注入的技术 (configuration injection). 这里, 根项目就像一个容器, subprojects 方法遍历这个容器的所有元素并且注入指定的配置 . 通过这种方法, 我们可以很容易的定义所有档案和通用依赖的内容清单:

*Example 7.12. 多项目构建 - 通用配置*

build.gradle

    subprojects {
        apply plugin: 'java'
        apply plugin: 'eclipse-wtp'

        repositories {
           mavenCentral()
        }

        dependencies {
            testCompile 'junit:junit:4.11'
        }

        version = '1.0'

        jar {
            manifest.attributes provider: 'gradle'
        }
    }

注意我们例子中, Java 插件被应用到了每一个子项目中plugin to each. 这意味着我们前几章看到的任务和属性都可以在子项目里被调用. 所以, 你可以通过在根目录里运行 **gradle build** 命令编译, 测试, 和 JAR 所有的项目.


