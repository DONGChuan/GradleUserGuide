# 跳过 tasks

Gradle 提供了好几种跳过一个任务的方式.

### 1. 使用判断条件 (predicate)

你可以使用 onlyIf() 方法来为一个任务加入判断条件. 就和 Java 里的 if 语句一样, 任务只有在条件判断为真时才会执行. 你通过一个闭包来实现判断条件. 闭包像变量一样传递任务, 如果任务应该被执行则返回真, 反之亦然. 判断条件在任务执行之前进行判断.

**例子 15.20. 使用判断条件跳过一个任务**

**build.gradle**

```
task hello << {
    println 'hello world'
}

hello.onlyIf { !project.hasProperty('skipHello') }
```

**gradle hello -PskipHello** 的输出

```
> gradle hello -PskipHello
:hello SKIPPED
BUILD SUCCESSFUL

Total time: 1 secs
```

### 2. 使用 StopExecutionException

如果想要跳过一个任务的逻辑并不能被判断条件通过表达式表达出来, 你可以使用 StopExecutionException. 如果这个异常是被一个任务要执行的动作抛出的, 这个动作之后的执行以及所有紧跟它的动作都会被跳过. 构建将会继续执行下一个任务.

**例子 15.21. 通过 StopExecutionException 跳过任务**

**build.gradle**

```
task compile << {
    println 'We are doing the compile.'
}

compile.doFirst {
    // Here you would put arbitrary conditions in real life.
    // But this is used in an integration test so we want defined behavior.
    if (true) { throw new StopExecutionException() }
}
task myTask(dependsOn: 'compile') << {
   println 'I am not affected'
}
```

**gradle -q myTask** 的输出

```
> gradle -q myTask
I am not affected
```

如果你直接使用 Gradle 提供的任务, 这项功能还是十分有用的. 它允许你为内建的任务加入条件来控制执行. [6]

### 3. 激活和注销 tasks

每一个任务都有一个已经激活的标记(enabled flag), 这个标记一般默认为真. 将它设置为假, 那它的任何动作都不会被执行.

**例子 15.22. 激活和注销 tasks**

**build.gradle**

```
task disableMe << {
    println 'This should not be printed if the task is disabled.'
}
disableMe.enabled = false
```

**gradle disableMe** 的输出

```
> gradle disableMe
:disableMe SKIPPED

BUILD SUCCESSFUL

Total time: 1 secs
```

