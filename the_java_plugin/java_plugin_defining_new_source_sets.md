### 22.7.2.定义一个新的source set
要定义一个新的源组，sourceSets {}块中引用它.下面是一个例子：

**例22.5.定义一个新的source set**

**build.gradle**

```
sourceSets {
    intTest
}
```

当你定义一个新的source set,java插件会为该source set添加一些如[Table 22.6, "Java plugin - source set dependency configurations"](https://docs.gradle.org/current/userguide/java_plugin.html#java_source_set_configurations)中所示的依赖配置关系.可以使用这些配置来定义source set的编译和运行时依赖。

**例22.6.定义source set的依赖**

**build.gradle**

```
sourceSets {
    intTest
}

dependencies {
    intTestCompile 'junit:junit:4.12'
    intTestRuntime 'org.ow2.asm:asm-all:4.0'
}
```

java插件增加了一些如[Table 22.2, "Java plugin - source set tasks"](https://docs.gradle.org/current/userguide/java_plugin.html#java_source_set_tasks)为该source set组装classes文件的任务,例如，对于一个叫intTest的source set，为此source set编译classes任务运行`gradle intTestClasses`完成。

**例22.7.编译一个source set**

`gradle intTestClasses`命令的输出

```
> gradle intTestClasses
:compileIntTestJava
:processIntTestResources
:intTestClasses

BUILD SUCCESSFUL

Total time: 1 secs
```
