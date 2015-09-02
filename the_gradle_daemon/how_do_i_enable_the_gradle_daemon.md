# 如何启动Gradle的守护进程

在使用Gradle命令行接口时,`--daemon`和`--no-daemon`命令行选项调用在单个构建时选择启用或禁用后台守护进程.通常,允许后台守护进程在一个环境中(例如一个用户账户)更为方便,可以使所有构建使用守护进程,而不需要记住`--daemon`开关.


有两种推荐的方式使守护进程持续与环境：

1. 通过环境变量 - 给`GRADLE_OPTS`环境变量添加`-Dorg.gradle.daemon=true`标识
2. 通过属性文件 - 给`<<GRADLE_USER_HOME>>/gradle.properties`文件添加`org.gradle.daemon=true`

> 注意:`<<GRADLE_USER_HOME>>`默认为`<<USER_HOME>>/.gradle`,`<<USER_HOME>>为当前用户home目录`,这个位置可以通过`-g`和`-gradle-user-home`命令行选项,以及由`GRADLE_USER_HOME`环境变量`org.gradle.user.home` JVM系统属性配置。

这两种方法有同样的效果,使用哪一个是由个人喜好.大多数Gradle用户选择第二个方式,给gradle.properties并添加条目.

在Windows中，该命令将使当前用户启用守护：

`(if not exist "%HOMEPATH%/.gradle" mkdir "%HOMEPATH%/.gradle") && (echo foo >> "%HOMEPATH%/.gradle/gradle.properties")`

在类Unix操作系统，以下的Bash shell命令将使当前用户启用守护进程：

`touch ~/.gradle/gradle.properties && echo "org.gradle.daemon=true" >> ~/.gradle/gradle.properties`

一旦以这种方式在构建环境中启用了守护进程,所有的构建将隐含一个守护进程.

