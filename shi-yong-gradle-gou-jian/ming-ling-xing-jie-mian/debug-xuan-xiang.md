# Debug 选项

`-?`,`-h`,`--help`

Shows a help message with all available CLI options.

`-v`,`--version`

Prints Gradle, Groovy, Ant, JVM, and operating system version information.

`-S`,`--full-stacktrace`

Print out the full \(very verbose\) stacktrace for any exceptions. See also[logging options](https://docs.gradle.org/current/userguide/command_line_interface.html#sec:command_line_logging).

`-s`,`--stacktrace`

Print out the stacktrace also for user exceptions \(e.g. compile error\). See also[logging options](https://docs.gradle.org/current/userguide/command_line_interface.html#sec:command_line_logging).

`--scan`

Create a[build scan](https://gradle.com/build-scans)with fine-grained information about all aspects of your Gradle build.

`-Dorg.gradle.debug=true`

Debug Gradle client \(non-Daemon\) process. Gradle will wait for you to attach a debugger at`localhost:5005`by default.

`-Dorg.gradle.daemon.debug=true`

Debug[Gradle Daemon](https://docs.gradle.org/current/userguide/gradle_daemon.html)process.

