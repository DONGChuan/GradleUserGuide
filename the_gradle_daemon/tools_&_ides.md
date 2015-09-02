# 工具和集成开发环境

Gradle工具API(参见[Chapter.65.Embedding Gradle](https://docs.gradle.org/current/userguide/embedding.html)),用于IDEs和其他工具整合Gradle,*总*是使用Gradle守护进程执行构建.如果你是从IDE内部执行构建,那么你是在使用守护进程,而且不需要在你的环境中允许Gradle守护进程.

但是，除非您已明确启用的Gradle守护进程在你的环境的,你在命令行中的构建不会使用摇篮守护进程。
