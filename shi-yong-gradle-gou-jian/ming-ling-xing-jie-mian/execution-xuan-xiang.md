# Execution 选项

The following options affect how builds are executed, by changing what is built or how dependencies are resolved.

`--include-build`

Run the build as a composite, including the specified build. See[Composite Builds](https://docs.gradle.org/current/userguide/composite_builds.html).

`--offline`

Specifies that the build should operate without accessing network resources. Learn more about[options to override dependency caching](https://docs.gradle.org/current/userguide/troubleshooting_dependency_resolution.html#sec:controlling_dependency_caching_command_line).

`--refresh-dependencies`

Refresh the state of dependencies. Learn more about how to use this in the[dependency management docs](https://docs.gradle.org/current/userguide/troubleshooting_dependency_resolution.html#sec:controlling_dependency_caching_command_line).

`--dry-run`

Run Gradle with all task actions disabled. Use this to show which task would have executed.



