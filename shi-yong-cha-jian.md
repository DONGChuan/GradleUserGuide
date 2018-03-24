# 使用插件

Gradle 包含各式各样的插件, 你可以在这里查阅它们 [Gradle 插件汇总](http://plugins.gradle.org/). 其中一个已经包含在发布的 Gradle 中的插件叫做`base`插件. 和另外一种核心类型 [`Zip`](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.bundling.Zip.html) 任务组合使用, 你可以将你的项目打包成一个 Zip 文件.

将`base`插件添加到`build.gradle`中. `plugins {}`语句块一定要放在文件的开头.

```
plugins {
    id "base"
}

... rest of the build file ...
```

现在添加一个任务用来打包项目的 `src`文件夹.

```
task zip(type: Zip, group: "Archive", description: "Archives sources in a zip file") {
    from "src"
}
```

执行该任务,`base`插件会在 `build/distributions` 文件夹中生成`basic-demo-1.0.zip` :

```
❯ ./gradlew zip
:zip

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```



