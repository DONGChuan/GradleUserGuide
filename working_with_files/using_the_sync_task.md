# 使用同步任务
同步任务 ( `Sync` ) 任务继承自复制任务 ( `Copy` ) , 当它执行时,它会复制源文件到目标目录中,然后从目标目录中的删除所有非复制的文件,这种方式非常有用,比如安装一个应用,创建一个文档的副本,或者维护项目的依赖关系副本.

下面有一个例子,维护 `build/libs` 目录下项目在运行时的依赖

**例 15.7 使用 Sync 任务复制依赖关系**

**build.gradle**

```
task libs(type: Sync) {
    from configurations.runtime
    into "$buildDir/libs"
}

```

