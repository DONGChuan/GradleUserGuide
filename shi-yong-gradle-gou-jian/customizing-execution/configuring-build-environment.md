Gradle provides multiple mechanisms for configuring behavior of Gradle itself and specific projects. The following is a reference for using these mechanisms.

When configuring Gradle behavior you can use these methods, listed in order of highest to lowest precedence \(first one wins\):

* [Command-line flags](https://docs.gradle.org/4.6/userguide/command_line_interface.html)such as`--build-cache`. These have precedence over properties and environment variables.

* [System properties](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_system_properties)such as`systemProp.http.proxyHost=somehost.org`stored in a`gradle.properties`file.

* [Gradle properties](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_configuration_properties)such as`org.gradle.caching=true`that are typically stored in a`gradle.properties`file in a project root directory or`GRADLE_USER_HOME`environment variable.

* [Environment variables](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:gradle_environment_variables)such as`GRADLE_OPTS`sourced by the environment that executes Gradle.

Aside from configuring the build environment, you can configure a given project build using[Project properties](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:project_properties)such as`-PreleaseType=final`.

## Gradle properties

Gradle provides several options that make it easy to configure the Java process that will be used to execute your build. While it’s possible to configure these in your local environment via`GRADLE_OPTS`or`JAVA_OPTS`, it is useful to store certain settings like JVM memory configuration and Java home location in version control so that an entire team can work with a consistent environment.

Setting up a consistent environment for your build is as simple as placing these settings into a`gradle.properties`file. The configuration is applied in following order \(if an option is configured in multiple locations the_last one wins_\):

* `gradle.properties`in project root directory.

* `gradle.properties`in`GRADLE_USER_HOME`directory.

* system properties, e.g. when`-Dgradle.user.home`is set on the command line.

The following properties can be used to configure the Gradle build environment:

`org.gradle.caching=(true,false)`

When set to true, Gradle will reuse task outputs from any previous build, when possible, resulting is much faster builds. Learn more about[using the build cache](https://docs.gradle.org/4.6/userguide/build_cache.html).

`org.gradle.caching.debug=(true,false)`

When set to true, individual input property hashes and the build cache key for each task are logged on the console. Learn more about[task output caching](https://docs.gradle.org/4.6/userguide/build_cache.html#sec:task_output_caching).

`org.gradle.configureondemand=(true,false)`

Enables incubating[configuration on demand](https://docs.gradle.org/4.6/userguide/multi_project_builds.html#sec:configuration_on_demand), where Gradle will attempt to configure only necessary projects.

`org.gradle.console=(auto,plain,rich,verbose)`

Customize console output coloring or verbosity. Default depends on how Gradle is invoked. See[command-line logging](https://docs.gradle.org/4.6/userguide/command_line_interface.html#sec:command_line_logging)for additional details.

`org.gradle.daemon=(true,false)`

When set to`true`the[Gradle Daemon](https://docs.gradle.org/4.6/userguide/gradle_daemon.html)is used to run the build. Default is`true`.

`org.gradle.daemon.idletimeout=(# of idle millis)`

Gradle Daemon will terminate itself after specified number of idle milliseconds. Default is`10800000`\(3 hours\).

`org.gradle.debug=(true,false)`

When set to`true`, Gradle will run the build with remote debugging enabled, listening on port 5005. Note that this is the equivalent of adding`-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=5005`to the JVM command line and will suspend the virtual machine until a debugger is attached. Default is`false`.

`org.gradle.java.home=(path to JDK home)`

Specifies the Java home for the Gradle build process. The value can be set to either a`jdk`or`jre`location, however, depending on what your build does, using a JDK is safer. A reasonable default is used if the setting is unspecified.

`org.gradle.jvmargs=(JVM arguments)`

Specifies the JVM arguments used for the Gradle Daemon. The setting is particularly useful for[configuring JVM memory settings](https://docs.gradle.org/4.6/userguide/build_environment.html#sec:configuring_jvm_memory)for build performance.

`org.gradle.logging.level=(quiet,warn,lifecycle,info,debug)`

When set to quiet, warn, lifecycle, info, or debug, Gradle will use this log level. The values are not case sensitive. The`lifecycle`level is the default. See[the section called “Choosing a log level”](https://docs.gradle.org/4.6/userguide/logging.html#sec:choosing_a_log_level).

`org.gradle.parallel=(true,false)`

When configured, Gradle will fork up to`org.gradle.workers.max`JVMs to execute projects in parallel. To learn more about parallel task execution, see[the Gradle performance guide](https://guides.gradle.org/performance/#parallel_execution).

`org.gradle.warning.mode=(all,none,summary)`

When set to`all`,`summary`or`none`, Gradle will use different warning type display. See[the section called “Logging options”](https://docs.gradle.org/4.6/userguide/command_line_interface.html#sec:command_line_logging)for details.

`org.gradle.workers.max=(max # of worker processes)`

When configured, Gradle will use a maximum of the given number of workers. Default is number of CPU processors. See also[performance command-line options](https://docs.gradle.org/4.6/userguide/command_line_interface.html#sec:command_line_performance).

The following example demonstrates usage of various properties.



**Example: Setting properties with a gradle.properties file**

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

Output of**`gradle -q -PcommandLineProjectProp=commandLineProjectPropValue -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps`**

```
>
 gradle -q -PcommandLineProjectProp=commandLineProjectPropValue -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps
commandLineProjectPropValue
gradlePropertiesValue
systemPropertyValue
envPropertyValue
systemValue

```

## System properties

Using the`-D`command-line option, you can pass a system property to the JVM which runs Gradle. The`-D`option of the`gradle`command has the same effect as the`-D`option of the`java`command.

You can also set system properties in`gradle.properties`files with the prefix`systemProp.`



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



