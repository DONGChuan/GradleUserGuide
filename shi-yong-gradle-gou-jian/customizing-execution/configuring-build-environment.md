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
        println System.properties[
'system'
]
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


**Example: Specifying system properties in`gradle.properties`**

```
systemProp.gradle.wrapperUser=myuser
systemProp.gradle.wrapperPassword=mypassword
```

The following system properties are available. Note that command-line options take precedence over system properties.

`gradle.wrapperUser=(myuser)`

Specify user name to download Gradle distributions from servers using HTTP Basic Authentication. Learn more in[the section called “Authenticated Gradle distribution download”](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html#sec:authenticated_download).

`gradle.wrapperPassword=(mypassword)`

Specify password for downloading a Gradle distribution using the Gradle wrapper.

`gradle.user.home=(path to directory)`

Specify the Gradle user home directory.

In a multi project build, “`systemProp.`” properties set in any project except the root will be ignored. That is, only the root project’s`gradle.properties`file will be checked for properties that begin with the “`systemProp.`” prefix.

## Environment variables

The following environment variables are available for the`gradle`command. Note that command-line options and system properties take precedence over environment variables.

`GRADLE_OPTS`

Specifies[command-line arguments](https://docs.gradle.org/4.6/userguide/command_line_interface.html)to use when starting the Gradle client. This can be useful for setting the properties to use when running Gradle.

`GRADLE_USER_HOME`

Specifies the Gradle user home directory \(which defaults to`$USER_HOME/.gradle`if not set\).

`JAVA_HOME`

Specifies the JDK installation directory to use.

## Project properties

You can add properties directly to your[`Project`](https://docs.gradle.org/4.6/dsl/org.gradle.api.Project.html)object via the`-P`command line option.

Gradle can also set project properties when it sees specially-named system properties or environment variables. If the environment variable name looks like`ORG_GRADLE_PROJECT`_`_prop`_`=somevalue`, then Gradle will set a`prop`property on your project object, with the value of`somevalue`. Gradle also supports this for system properties, but with a different naming pattern, which looks like`org.gradle.project.`_`prop`_. Both of the following will set the`foo`property on your Project object to`"bar"`.



**Example: Setting a project property via gradle.properties**

```
org.gradle.project.foo=bar
```



**Example: Setting a project property via environment variable**

```
ORG_GRADLE_PROJECT_foo=bar
```

The properties file in the user’s home directory has precedence over property files in the project directories.

This feature is very useful when you don’t have admin rights to a continuous integration server and you need to set property values that should not be easily visible. Since you cannot use the`-P`option in that scenario, nor change the system-level configuration files, the correct strategy is to change the configuration of your continuous integration build job, adding an environment variable setting that matches an expected pattern. This won’t be visible to normal users on the system.

You can access a project property in your build script simply by using its name as you would use a variable.

If a project property is referenced but does not exist, an exception will be thrown and the build will fail.

You should check for existence of optional project properties before you access them using the[`Project.hasProperty(java.lang.String)`](https://docs.gradle.org/4.6/dsl/org.gradle.api.Project.html#org.gradle.api.Project:hasProperty%28java.lang.String%29)method.

## Configuring JVM memory

Gradle defaults to 1024 megabytes maximum heap per JVM process \(`-Xmx1024m`\), however, that may be too much or too little depending on the size of your project. There are many JVM options \(this[blog post on Java performance tuning](https://dzone.com/articles/java-performance-tuning)and[this reference](http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html)may be helpful\).

You can adjust JVM options for Gradle in the following ways:

The`JAVA_OPTS`environment variable is used for the Gradle client, but not forked JVMs.



**Example: Changing JVM settings for Gradle client JVM**

```
JAVA_OPTS="-Xmx2g -XX:MaxPermSize=256m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8"
```

You need to use the`org.gradle.jvmargs`Gradle property to configure JVM settings for the[Gradle Daemon](https://docs.gradle.org/4.6/userguide/gradle_daemon.html).



**Example: Changing JVM settings for forked Gradle JVMs**

```
org.gradle.jvmargs=-Xmx2g -XX:MaxPermSize=256m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
```

Many settings \(like the Java version and maximum heap size\) can only be specified when launching a new JVM for the build process. This means that Gradle must launch a separate JVM process to execute the build after parsing the various`gradle.properties`files.

When running with the[Gradle Daemon](https://docs.gradle.org/4.6/userguide/gradle_daemon.html), a JVM with the correct parameters is started once and reused for each daemon build execution. When Gradle is executed without the daemon, then a new JVM must be launched for every build execution, unless the JVM launched by the Gradle start script happens to have the same parameters.

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

## Configuring a task using project properties

It’s possible to change the behavior of a task based on project properties specified at invocation time.

Suppose you’d like to ensure release builds are only triggered by CI. A simple way to handle this is through an`isCI`project property.



**Example: Prevent releasing outside of CI**

`build.gradle`

```
task performRelease {
    doLast {
        
if
 (project.hasProperty(
"isCI"
)) {
            println(
"Performing release actions"
)
        } 
else
 {
            
throw
new
 InvalidUserDataException(
"Cannot perform release outside of CI"
)
        }
    }
}

```

Output of**`gradle performRelease -PisCI=true --quiet`**

```
>
 gradle performRelease -PisCI=true --quiet
Performing release actions

```

## Accessing the web through a HTTP proxy

Configuring an HTTP or HTTPS proxy \(for downloading dependencies, for example\) is done via standard JVM system properties. These properties can be set directly in the build script; for example, setting the HTTP proxy host would be done with`System.setProperty('http.proxyHost', 'www.somehost.org')`. Alternatively, the properties can be[specified in gradle.properties](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_configuration_properties).



**Example: Configuring an HTTP proxy using`gradle.properties`**

```
systemProp.http.proxyHost=www.somehost.org
systemProp.http.proxyPort=8080
systemProp.http.proxyUser=userid
systemProp.http.proxyPassword=password
systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost
```

There are separate settings for HTTPS.



**Example: Configuring an HTTPS proxy using`gradle.properties`**

```
systemProp.https.proxyHost=www.somehost.org
systemProp.https.proxyPort=8080
systemProp.https.proxyUser=userid
systemProp.https.proxyPassword=password
systemProp.https.nonProxyHosts=*.nonproxyrepos.com|localhost
```

You may need to set other properties to access other networks. Here are 2 references that may be helpful:

* [ProxySetup.java in the Ant codebase](https://git-wip-us.apache.org/repos/asf?p=ant.git;a=blob;f=src/main/org/apache/tools/ant/util/ProxySetup.java;hb=HEAD)

* [JDK 7 Networking Properties](http://download.oracle.com/javase/7/docs/technotes/guides/net/properties.html)

### NTLM Authentication

If your proxy requires NTLM authentication, you may need to provide the authentication domain as well as the username and password. There are 2 ways that you can provide the domain for authenticating to a NTLM proxy:

* Set the`http.proxyUser`system property to a value like_`domain`_`/`_`username`_.

* Provide the authentication domain via the`http.auth.ntlm.domain`system property.



