# 配置构建环境

Gradle 提供数种机制来配置 Gradle 自身以及特殊的项目.

但配置 Gradle 的行为的时候, 你可以使用下列方法,  按优先权从高到低排列 \(第一个最高\):

* [Command-line flags](https://docs.gradle.org/4.6/userguide/command_line_interface.html)比如`--build-cache`. 它比属性和环境变量有更高的优先权.

* [System 属性](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_system_properties)比如`gradle.properties`文件里的`systemProp.http.proxyHost=somehost.org`.

* [Gradle 属性](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_configuration_properties)比如项目根目录`gradle.properties`文件里的`org.gradle.caching=true`.

* [环境变量](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_environment_variables)比如环境变量`GRADLE_OPTS`.

除了配置构建环境, 你可以使用[项目属性]来配置一个给定的项目(https://docs.gradle.org/4.6/userguide/build_environment.html#sec:project_properties), 比如`-PreleaseType=final`.

## Gradle 属性

Gradle 提供数种选项来简化配置执行构建的 Java 进程. 当然, 你也可以通过你的本地环境变量`GRADLE_OPTS`或`JAVA_OPTS`来配置, 然后通过 Gradle 属性来设置, 这些设置就可以储存在版本控制里(比如git), 比如 JVM memory 配置和 Java home location, 这样的话, 一整个团队都可以保持一致的构建环境.

你只需要将这些设置统一放在`gradle.properties`文件里就可以保持一致的构建环境. 所有的配置将会以下列的顺序来实行:

* 项目根目录的`gradle.properties`.

* `GRADLE_USER_HOME` 目录里的 `gradle.properties`.

* system 属性, e.g. 当在命令行设置`-Dgradle.user.home`.

下列属性可以被用来配置 Gradle 构建环境:

`org.gradle.caching=(true,false)`

当设置为真, Gradle 会重复使用之前任务的输出, 这样执行更加高效. 可以阅读[使用构建缓存](https://docs.gradle.org/4.6/userguide/build_cache.html)了解更多.

`org.gradle.caching.debug=(true,false)`

当设置为真, 每一个独立的输入属性哈希值以及每一个任务的构建缓存的键值都会以日志的形式显示在控制台. 可以阅读[任务输出缓存](https://docs.gradle.org/4.6/userguide/build_cache.html#sec:task_output_caching)了解更多.

`org.gradle.configureondemand=(true,false)`

Enables incubating[configuration on demand](https://docs.gradle.org/4.6/userguide/multi_project_builds.html#sec:configuration_on_demand), where Gradle will attempt to configure only necessary projects.

`org.gradle.console=(auto,plain,rich,verbose)`

自定义控制台输出的颜色或显示的内容级别. 查看[命令行日志](https://docs.gradle.org/4.6/userguide/command_line_interface.html#sec:command_line_logging)了解更多.

`org.gradle.daemon=(true,false)`

当设置为真, [Gradle 守护进程](https://docs.gradle.org/4.6/userguide/gradle_daemon.html)就会被使用来构建. 默认为真.

`org.gradle.daemon.idletimeout=(# of idle millis)`

Gradle 守护进程会在空闲指定的毫秒后结束自己. 默认是`10800000`\(3 个小时\).

`org.gradle.debug=(true,false)`

当设置为真, Gradle 将会激活远程调试来构建, 监听 5005 端口. 注意, 这其实就是添加 `-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=5005` 选项到 JVM 命令行, 将会暂停虚拟机直到一个调试器关联起来. 默认是`false`.

`org.gradle.java.home=(path to JDK home)`

为 Gradle 构建进程指定 Java home. 既可以是`jdk`也可以是`jre`的位置, 然后, 根据你的构建要做什么, 使用 JDK 可能更安全.

`org.gradle.jvmargs=(JVM arguments)`

指定 Gradle 守护进程使用的 JVM 参数. 这个设置对于[配置 JVM memory 设置](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:configuring_jvm_memory)在构建性能方面是十分有用的.

`org.gradle.logging.level=(quiet,warn,lifecycle,info,debug)`

设置 Gradle 日志级别. 参数大小写都可以. 默认是`lifecycle`级别. 查阅[选择一个日志级别](https://docs.gradle.org/4.6/userguide/logging.html#sec:choosing_a_log_level)了解更多.

`org.gradle.parallel=(true,false)`

设置是否并行执行多个项目. 查看[Gradle 性能指南](https://guides.gradle.org/performance/#parallel_execution)了解更多关于并行任务执行的知识.

`org.gradle.warning.mode=(all,none,summary)`

设置 Gradle 不同的警告类型. 查看[日志选项了解更多](https://docs.gradle.org/4.6/userguide/command_line_interface.html#sec:command_line_logging).

`org.gradle.workers.max=(max # of worker processes)`

设置 Gradle 可以使用的最大 workers 数量. 默认是 CPU 处理器的数量.查看[性能命令行选项](https://docs.gradle.org/4.6/userguide/command_line_interface.html#sec:command_line_performance)了解更多.

下面列举几个使用这些属性的例子


**Example: gradle.properties 文件中设置属性**

`gradle.properties`

```
gradlePropertiesProp=gradlePropertiesValue
sysProp=shouldBeOverWrittenBySysProp
envProjectProp=shouldBeOverWrittenByEnvProp
systemProp.system=systemValue

```

`build.gradle`

```
task printProps {
    doLast {
        println commandLineProjectProp
        println gradlePropertiesProp
        println systemProjectProp
        println envProjectProp
        println System.properties['system']
    }
}

```

**`gradle -q -PcommandLineProjectProp=commandLineProjectPropValue -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps`**的输出:

```
> gradle -q -PcommandLineProjectProp=commandLineProjectPropValue -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps
commandLineProjectPropValue
gradlePropertiesValue
systemPropertyValue
envPropertyValue
systemValue

```

## 系统属性

通过使用`-D`命令行选项, 你可以传递一个系统属性给运行 Gradle 的 JVM. 这个 Gradle 的`-D`选项的效果其实和`java`命令的`-D`是一样的.

你可以在`gradle.properties`文件中通过前缀`systemProp.`来使用系统属性


**Example: 在`gradle.properties`文件中设置系统属性**

```
systemProp.gradle.wrapperUser=myuser
systemProp.gradle.wrapperPassword=mypassword
```

有下列可用的系统属性. 请注意, 命令行选项的优先级是在系统属性之上的.

`gradle.wrapperUser=(myuser)`

指定使用 HTTP Basic Authentication 从服务器上下载 Gradle 发布所使用的用户名. 查阅[Authenticated Gradle distribution download](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html#sec:authenticated_download)了解更多.

`gradle.wrapperPassword=(mypassword)`

指定使用 Gradle wrapper 下载 Gradle 发布的密码.

`gradle.user.home=(path to directory)`

指定 Gradle 用户 home 文件夹.

在一个多项目构建当中, “`systemProp.`” 属性只有在根项目中才会起作用. 也就是说只有在`gradle.properties`文件的根项目中才会检查有 “`systemProp.`” 前缀的属性.

## 环境变量

下面的环境变量可以在 Gradle 命令中使用. 请注意, 命令行选项以及系统选项的优先级在环境变量之上.

`GRADLE_OPTS`

指定要使用的[命令行参数](https://docs.gradle.org/4.6/userguide/command_line_interface.html).

`GRADLE_USER_HOME`

指定 Gradle 用户 home 文件夹 \(如果没有设置的话, 默认`$USER_HOME/.gradle`\).

`JAVA_HOME`

指定 JDK 安装目录.

## 项目属性

你可以通过`-P`命令行选项直接在你的[`项目`](https://docs.gradle.org/4.6/dsl/org.gradle.api.Project.html)中添加属性.

当 Gradle 发现特殊命名的系统属性或者环境变量, 就会将它们设置为项目属性. 比如环境变量的名称为`ORG_GRADLE_PROJECT`_`_prop`_`=somevalue`, Gradle 就会设置一个`prop`属性到你的项目上, 并赋值为`somevalue`. 对于系统属性, Gradle 也会做类似的处理, 但是使用不同的名称模式, 有点像`org.gradle.project.`_`prop`_. 下面的 2 个列子都会设置一个 `foo` 属性到你的项目的 `"bar"` 对象上.


**Example: 通过 gradle.properties 设置项目属性***

```
org.gradle.project.foo=bar
```

**Example: 通过环境变量设置项目属性***

```
ORG_GRADLE_PROJECT_foo=bar
```

用户 home 文件夹下的属性文件优先级要比项目文件夹下的属性文件优先级高.

这个特点在某些场合下十分有用, 比如你没有 ADMIN 权限, 没法操作一个持续集成的服务器, 而你需要设置一个不可见或者说不是那么容易可见的属性值. 这种情况下, 你既不能使用 `-P` 选项, 也不能改变系统级别的配置文件, 正确的方式是改变你的持续集成构建工作的配置, 添加一个环境变量. 对于系统上的普通用户是不可见的.

你可以轻易的使用你的构建脚本里的一个项目属性, 只需要使用它的名称作为变量即可.

如果一个项目属性被引用了却不存在, 构建就会失败并抛出一个异常.

你应该在使用可选的项目属性之前使用[`Project.hasProperty(java.lang.String)`](https://docs.gradle.org/4.6/dsl/org.gradle.api.Project.html#org.gradle.api.Project:hasProperty%28java.lang.String%29)方法检查它们是否存在.

## 配置 JVM 内存

Gradle 默认提供最大 1024 MB 的内存给 JVM 进程 \(`-Xmx1024m`\), 然而, 1024MB 对于你的项目来说可能太大了或者完全不够. JVM 提供了许多选项, 阅读 \([blog post on Java performance tuning](https://dzone.com/articles/java-performance-tuning)和[this reference](http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html)\)了解更多.

你可以通过下列方法调整 JVM 选项:

环境变量 `JAVA_OPTS` is used for the Gradle client, but not forked JVMs.


**Example: Changing JVM settings for Gradle client JVM**

```
JAVA_OPTS="-Xmx2g -XX:MaxPermSize=256m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8"
```

你需要使用 `org.gradle.jvmargs` 来配置 JVM [保护进程](https://docs.gradle.org/4.6/userguide/gradle_daemon.html)的设置.


**Example: Changing JVM settings for forked Gradle JVMs**

```
org.gradle.jvmargs=-Xmx2g -XX:MaxPermSize=256m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
```

许多设置 \(比如 Java 版本和最大的堆栈大小\) 只能在启动一个新的 JVM 来构建的时候才能指定. 这意味着 Gradle 必须启动一个独立的 JVM 进程来分析新改变的`gradle.properties`文件去构建.

当运行[保护进程](https://docs.gradle.org/4.6/userguide/gradle_daemon.html)的时候, 只要参数正确, JVM 启动一次后就可以给每一个保护进程的构建重复使用. 如果 Gradle 没有使用保护进程执行, 那么一个新的 JVM 就必须被启用来执行每一个构建, 除非被 Gradle 开始脚本启动的 JVM 有完全一样的参数.

Certain tasks in Gradle also fork additional JVM processes, like the`test`task when using[`Test.setMaxParallelForks(int)`](https://docs.gradle.org/4.6/javadoc/org/gradle/api/tasks/testing/Test.html#setMaxParallelForks-int-)for JUnit or TestNG tests. You must configure these through the tasks themselves.


**Example: Set Java compile options for**[**`JavaCompile`**](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.compile.JavaCompile.html)**tasks**

```
apply plugin: "java"

tasks.withType(JavaCompile) {
    options.compilerArgs += ["-Xdoclint:none", "-Xlint:none", "-nowarn"]
}
```

See other examples in the[`Test`](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.testing.Test.html)API documentation and[test execution in the Java plugin reference](https://docs.gradle.org/4.6/userguide/java_plugin.html#sec:test_execution).

[Build scans](https://scans.gradle.com/)will tell you information about the JVM that executed the build when you use the`--scan`option.

[![](https://docs.gradle.org/4.6/userguide/img/build-scan-infrastructure.png "Build Environment in build scan")](https://scans.gradle.com/s/sample/cpp-parallel/infrastructure)

## 使用项目属性配置任务

可以在调用时通过项目属性来改变任务的行为.

假设你想要确保发布构建只会被 CI 触发. 最简单的方法就是使用项目属性 `isCI`.


**Example: 防止 CI 之外的发布**

`build.gradle`

```
task performRelease {
    doLast {
        if (project.hasProperty("isCI")) {
            println("Performing release actions")
        } else {
            throw new InvalidUserDataException("Cannot perform release outside of CI")
        }
    }
}

```

**`gradle performRelease -PisCI=true --quiet`**的输出:

```
> gradle performRelease -PisCI=true --quiet
Performing release actions

```

## 通过 HTTP 代理进入网页

配置一个 HTTP 或 HTTPS 代理 \(比如为了下载依赖\), 可以通过标准的 JVM 系统属性来实现. 这些属性都可以直接在构建脚本里设置, 通过 `System.setProperty('http.proxyHost', 'www.somehost.org')` 来设置 HTTP 代理. 另外一种选择, 请阅读[specified in gradle.properties](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_configuration_properties).


**Example: 使用`gradle.properties`配置 HTTP 代理**

```
systemProp.http.proxyHost=www.somehost.org
systemProp.http.proxyPort=8080
systemProp.http.proxyUser=userid
systemProp.http.proxyPassword=password
systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost
```

对于 HTTPS:


**Example: 使用`gradle.properties`配置 HTTPS 代理**

```
systemProp.https.proxyHost=www.somehost.org
systemProp.https.proxyPort=8080
systemProp.https.proxyUser=userid
systemProp.https.proxyPassword=password
systemProp.https.nonProxyHosts=*.nonproxyrepos.com|localhost
```

你也许需要设置其他的属性来进入其他特别的网络, 下面 2 篇文章将非常有帮助:

* [ProxySetup.java in the Ant codebase](https://git-wip-us.apache.org/repos/asf?p=ant.git;a=blob;f=src/main/org/apache/tools/ant/util/ProxySetup.java;hb=HEAD)

* [JDK 7 Networking Properties](http://download.oracle.com/javase/7/docs/technotes/guides/net/properties.html)

### NTLM Authentication

If your proxy requires NTLM authentication, you may need to provide the authentication domain as well as the username and password. There are 2 ways that you can provide the domain for authenticating to a NTLM proxy:

* Set the`http.proxyUser`system property to a value like_`domain`_`/`_`username`_.

* Provide the authentication domain via the`http.auth.ntlm.domain`system property.



