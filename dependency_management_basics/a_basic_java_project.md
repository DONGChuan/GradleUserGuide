# 声明你的依赖

让我们看一下一些依赖的声明. 下面是一个基础的构建脚本:

*例子 7.1. 声明依赖*

*build.gradle*

    apply plugin: 'java'

    repositories {
        mavenCentral()
    }

    dependencies {
        compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'
        testCompile group: 'junit', name: 'junit', version: '4.+'
    }

这里发生了什么? 这个构建脚本声明 Hibernate core 3.6.7.Final 被用来编译项目的源代码. By implication, 在运行阶段同样也需要 Hibernate core 和它的依赖. 构建脚本同样声明了需要 junit >= 4.0 的版本来编译项目测试. 它告诉 Gradle 到 Maven 库里找任何需要的依赖. 接下来的部分会具体说明.


