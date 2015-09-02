# 使用同步任务
The Sync task extends the Copy task. When it executes, it copies the source files into the destination directory, and then removes any files from the destination directory which it did not copy. This can be useful for doing things such as installing your application, creating an exploded copy of your archives, or maintaining a copy of the project's dependencies.

Here is an example which maintains a copy of the project's runtime dependencies in the build/libs directory.

同步任务 ( `Sync` ) 任务继承自复制任务 ( `Copy` ) , 当它执行时,它会复制源文件到目标目录中,然后从目标目录中的删除所有非复制的文件,这种方式非常有用,比如安装一个应用,创建一个文档的副本,或者保持项目的依赖关系副本.

下面有一个例子,保持 `build/libs` 目录下项目在运行时的依赖

**例 15.7 使用 Sync 任务复制依赖**

**build.gradle**

```
task libs(type: Sync) {
    from configurations.runtime
    into "$buildDir/libs"
}

```

