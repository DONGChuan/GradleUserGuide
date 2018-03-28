# Performance 选项

Try these options when optimizing build performance. Learn more about[improving performance of Gradle builds here](https://guides.gradle.org/performance/).

Many of these options can be specified in`gradle.properties`so command-line flags are not necessary. See the[configuring build environment guide](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties).

`--build-cache`

,

`--no-build-cache`

Toggles the[Gradle build cache](https://docs.gradle.org/current/userguide/build_cache.html). Gradle will try to reuse outputs from previous builds._Default is off_.

`--configure-on-demand`

,

`--no-configure-on-demand`

Toggles[Configure-on-demand](https://docs.gradle.org/current/userguide/multi_project_builds.html#sec:configuration_on_demand). Only relevant projects are configured in this build run._Default is off_.

`--max-workers`

Sets maximum number of workers that Gradle may use._Default is number of processors_.

`--parallel`

,

`--no-parallel`

Build projects in parallel. For limitations of this option please see[the section called “Parallel project execution”](https://docs.gradle.org/current/userguide/multi_project_builds.html#sec:parallel_execution)._Default is off_.

`--profile`

Generates a high-level performance report in the`$buildDir/reports/profile`directory.`--scan`is preferred.

`--scan`

Generate a build scan with detailed performance diagnostics.

![](https://docs.gradle.org/current/userguide/img/gradle-core-test-build-scan-performance.png "Build Scan performance report")

### Gradle daemon options

You can manage the[Gradle Daemon](https://docs.gradle.org/current/userguide/gradle_daemon.html)through the following command line options.

`--daemon`

,

`--no-daemon`

Use the[Gradle Daemon](https://docs.gradle.org/current/userguide/gradle_daemon.html)to run the build. Starts the daemon if not running or existing daemon busy._Default is on_.

`--foreground`

Starts the Gradle Daemon in a foreground process.

`--status`

\(Standalone command\)

Run`gradle --status`to list running and recently stopped Gradle daemons. Only displays daemons of the same Gradle version.

`--stop`

\(Standalone command\)

Run`gradle --stop`to stop all Gradle Daemons of the same version.

`-Dorg.gradle.daemon.idletimeout=(number of milliseconds)`

Gradle Daemon will stop itself after this number of milliseconds of idle time._Default is 10800000_\(3 hours\).

