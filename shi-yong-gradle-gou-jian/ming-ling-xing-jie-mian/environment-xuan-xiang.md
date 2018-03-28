# Environment 选项

You can customize many aspects about where build scripts, settings, caches, and so on through the options below. Learn more about customizing your[build environment](https://docs.gradle.org/current/userguide/build_environment.html).

`-b`,`--build-file`

Specifies the build file. For example:`gradle --build-file=foo.gradle`. The default is`build.gradle`, then`build.gradle.kts`, then`myProjectName.gradle`.

`-c`,`--settings-file`

Specifies the settings file. For example:`gradle --settings-file=somewhere/else/settings.gradle`

`-g`,`--gradle-user-home`

Specifies the Gradle user home directory. The default is the`.gradle`directory in the user’s home directory.

`-p`,`--project-dir`

Specifies the start directory for Gradle. Defaults to current directory.

`--project-cache-dir`

Specifies the project-specific cache directory. Default value is`.gradle`in the root project directory.

`-u`,`--no-search-upward`\(deprecated\)

Don’t search in parent directories for a`settings.gradle`file.

`-D`,`--system-prop`

Sets a system property of the JVM, for example`-Dmyprop=myvalue`. See[the section called “System properties”](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_system_properties).

`-I`,`--init-script`

Specifies an initialization script. See[Initialization Scripts](https://docs.gradle.org/current/userguide/init_scripts.html).

`-P`,`--project-prop`

Sets a project property of the root project, for example`-Pmyprop=myvalue`. See[the section called “Project properties”](https://docs.gradle.org/current/userguide/build_environment.html#sec:project_properties).

`-Dorg.gradle.jvmargs`

Set JVM arguments.

`-Dorg.gradle.java.home`

Set JDK home dir.

