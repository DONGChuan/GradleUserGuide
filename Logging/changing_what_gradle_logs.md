# 改变Gradle的记录内容

你可以用自己的内容取代大部分摇篮的UI记录.你可以这样做，例如，如果你想以某种方式定制UI,如:记录更多或更少的信息，或更改log的格式.你可以使用[Gradle.useLogger()](https://docs.gradle.org/current/dsl/org.gradle.api.invocation.Gradle.html#org.gradle.api.invocation.Gradle:useLogger(java.lang.Object))方法替换日志.可从一个构建脚本或初始化脚本，或通过嵌入API替换.注意，这会完全禁用Gradle的默认输出.下面的初始化脚本改变任务执行和完成构建后日志的输出.

**例 17.6.定制Gradle logs**

**init.gradle**

```gradle
useLogger(new CustomEventLogger())

class CustomEventLogger extends BuildAdapter implements TaskExecutionListener {

    public void beforeExecute(Task task) {
        println "[$task.name]"
    }

    public void afterExecute(Task task, TaskState state) {
        println()
    }

    public void buildFinished(BuildResult result) {
        println 'build completed'
        if (result.failure != null) {
            result.failure.printStackTrace()
        }
    }
}
```
`gradle -I init.gradle build`的输出

> \> gradle -I init.gradle build
> [compile]
> compiling source
>
> [testCompile]
> compiling test source
>
> [test]
> running unit tests
>
> [build]
>
> build completed

你的`logger`可以实现下面列出的任何监听器接口.仅它实现接口被替换,其他接口保持不变。你可以在[Section 56.6, “Responding to the lifecycle in the build script”](https://docs.gradle.org/current/userguide/build_lifecycle.html#build_lifecycle_events)中找到更多相关信息.

+ [BuildListener](https://docs.gradle.org/current/javadoc/org/gradle/BuildListener.html)
+ [ProjectEvaluationListener](https://docs.gradle.org/current/javadoc/org/gradle/api/ProjectEvaluationListener.html)
+ [TaskExecutionGraphListener](https://docs.gradle.org/current/javadoc/org/gradle/api/execution/TaskExecutionGraphListener.html)
+ [TaskExecutionListener](https://docs.gradle.org/current/javadoc/org/gradle/api/execution/TaskExecutionListener.html)
+ [TaskActionListener](https://docs.gradle.org/current/javadoc/org/gradle/api/execution/TaskActionListener.html)
