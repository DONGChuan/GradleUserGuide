# Execution 选项

下列的选项将影响构建的执行, 比如构建什么以及如何处理依赖.

`--include-build`

以合成的方式运行一个构建, 指定一个构建脚本. 阅读[合成构建](https://docs.gradle.org/current/userguide/composite_builds.html)了解更多.

`--offline`

指定构建是离线模式的, 即不能使用网络上的资源[覆写依赖缓存的选项](https://docs.gradle.org/current/userguide/troubleshooting_dependency_resolution.html#sec:controlling_dependency_caching_command_line)了解更多.

`--refresh-dependencies`

刷新依赖的状态. 阅读[依赖管理了解更多](https://docs.gradle.org/current/userguide/troubleshooting_dependency_resolution.html#sec:controlling_dependency_caching_command_line).

`--dry-run`

运行 Gradle 的时候禁用所有任务的动作. 我们可以使用这个选项来查看我们定义的依赖或者插件里的依赖是否都正确定义了. 我们可以在输出中查看都有哪些依赖被执行了.