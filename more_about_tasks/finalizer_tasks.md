# 终止 tasks

>终止任务是一个正在开发的功能.

这里的终止任务并不是指终止一个任务, 而是指一个无论运行结果如何最后都会被执行的任务.

**例子 15.27. 加入一个任务终止器**

**build.gradle**

```
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}

taskX.finalizedBy taskY
```

**gradle -q taskX** 的输出

```
> gradle -q taskX
taskX
taskY
```

即使要终止的任务失败了, 终止任务仍会继续执行.

**例子 14.28. 当任务失败时k**

**build.gradle**
```
task taskX << {
    println 'taskX'
    throw new RuntimeException()
}
task taskY << {
    println 'taskY'
}

taskX.finalizedBy taskY
```
**gradle -q taskX** 的输出
```
> gradle -q taskX
taskX
taskY
```

另外, 如果要终止的任务并没有被执行 (比如上一节讲的 up-to-date) 那么终止任务并不会执行.

当构建创建了一个资源, 无论构建失败或成功时这个资源必须被清除的时候, 终止任务就非常有用.

要使用终止任务, 你必须使用 [Task.finalizedBy()](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html#org.gradle.api.Task:finalizedBy(java.lang.Object[])) 方法. 一个任务的实例, 任务的名称, 或者任何 Task.dependsOn() 可以接收的输入都可以作为这个任务的输入.
