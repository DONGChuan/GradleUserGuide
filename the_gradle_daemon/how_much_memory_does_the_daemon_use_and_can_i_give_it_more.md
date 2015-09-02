# 守护进程占用多大内存并且能不能给它更大的内存?

如果需求构建环境没有指定最大堆内存,守护进程会使用多达1G的堆内存.它将会使用默认的JVM的最小堆内存.1G内存足够应付大多数构建.有数百个子项的构建,大量配置或者源码需求,或者要求有更好的表现,则需要更多地内存

为了提高守护进程可以使用的内存,指定相应的标志作为需求构建环境的一部分,请参见[Chapter 20. The Build Environment](https://docs.gradle.org/current/userguide/build_environment.html)的详细信息.


