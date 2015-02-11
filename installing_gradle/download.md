# 下载与安装

你可以从 [Gradle网站](http://www.gradle.org/downloads) 下载任意一个已经发布的版本

# 解压缩

Gradle 发布的版本为 ****ZIP 格式. 所有文件包含:

* Gradle 的二进制文件.
* 用户指南 (HTML 和 PDF).
* DSL参考指南.
* API文档 (Javadoc和 Groovydoc).
* 扩展的例子,包括用户指南中引用的实例，以及一些更复杂的实例来帮助用户构建自己的build.
* 二进制源码.此代码仅供参考.

# 设置环境变量

然后我们需要设置环境变量

1. 添加一个 **GRADLE_HOME** 环境变量来指明 Gradle 的安装路径
2. 添加 **GRADLE_HOME/bin** 到您的 *PATH* 环境变量中. 通常, 这样已经足够运行Gradle了.

**扩充教程**

**Max OX**:

假设您下载好的 Gradle 文件在 /Users/UFreedom/gradle 目录

    1.vim ~/.bash_profile

    2.添加下面内容：
    export GRADLE_HOME = /Users/UFreedom/gradle
    export export PATH=$PATH:$GRADLE_HOME/bin

    3.source ~/.brash_profile

实际配置中，您需要将上面的目录换成您的 Gradle 文件在系统中的目录.




# 运行并测试您的安装

然后我们需要测试 Gradle 是否已经正确安装. 您可以通过在控制台输入 **gradle** 命令来运行Gradle. 通过 **gradle -v** 命令来检测Gradle是否已经正确安装. 如果正确安装，会输出Gradle版本信息以及本地的配置环境  ( groovy 和 JVM 版本等). 显示的版本信息应该与您所下载的 gradle 版本信息相匹配.

