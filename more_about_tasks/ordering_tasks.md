# 给 tasks 排序

>任务的排序功能正在测试和优化. 请注意, 这项功能在 Gradle 之后的版本里可能会改变.

在某些情况下, 我们希望能控制任务的的执行顺序, 这种控制并不是向上一张那样去显示地加入依赖关系. 最主要的区别是我们设定的排序规则不会影响那些要被执行的任务, 只是影响执行的顺序本身. 好吧, 我知道可能有点抽象.

我们来看看以下几种有用的场景:

* 执行连续的任务: eg. 'build' 从来不会在 'clean' 之前执行.
* 在 build 的一开始先运行构建确认 (build validations): eg. 在正式的发布构建前先确认我的证书是正确的.
* 在运行长时间的检测任务前先运行快速的检测任务来获得更快的反馈: eg. 单元测试总是应该在集成测试之前被执行.
* 一个聚集 (aggregates) 某种特定类型的所有任务结果的任务: eg. 测试报告任务 (test report task) 包含了所有测试任务的运行结果.

目前, 有 2 种可用的排序规则: **"must run after"** 和 **"should run after"**.

当你使用 “must run after” 时即意味着 taskB 必须总是在 taskA 之后运行, 无论 taskA 和 taskB 是否将要运行:

```
taskB.mustRunAfter(taskA)
```

"should run after" 规则其实和 "must run after" 很像, 只是没有那么的严格, 在 2 种情况下它会被忽略:

1. 使用规则来阐述一个执行的循环.
2. 当并行执行并且一个任务的所有依赖除了 “should run after” 任务其余都满足了, 那么这个任务无论它的 “should run after” 依赖是否执行, 它都可以执行. (编者: 翻译待商榷, 提供具体例子)

总之, 当要求不是那么严格时, “should run after” 是非常有用的.

即使有目前的这些规则, 我们仍可以执行 taskA 而不管 taskB, 反之亦然.

**例子 15.14. 加入 'must run after' **

**build.gradle**
```
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}
taskY.mustRunAfter taskX
```

**gradle -q taskY taskX** 的输出

```
> gradle -q taskY taskX
taskX
taskY
```

**例子 15.15. 加入 'should run after'**

**build.gradle**

```
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}
taskY.shouldRunAfter taskX
```

**gradle -q taskY taskX** 的输出

```
> gradle -q taskY taskX
taskX
taskY
```

在上面的例子里, 我们仍可以直接执行 taskY 而不去 taskX :

**例子 15.16. 任务排序不影响任务执行**

**gradle -q taskY** 的输出

```
> gradle -q taskY
taskY
```

为了在 2 个任务间定义 “must run after” 或者 “should run after” 排序, 我们需要使用 Task.mustRunAfter() 和 Task.shouldRunAfter() 方法. 这些方法接收一个任务的实例,　任务的名字或者任何 Task.dependsOn()可以接收的输入.

注意 “B.mustRunAfter(A)” 或者 “B.shouldRunAfter(A)” 并不影响任何任务间的执行依赖:

* tasks A 和 B 可以被独立的执行. 排序规则只有当 2 个任务同时执行时才会被应用.
* 在运行时加上 --continue, 当 A 失败时 B 仍然会执行.

之前提到过, “should run after” 规则在一个执行循环中将被忽略:

**例子 15.17. 'should run after' 任务的忽略**

**build.gradle**

```
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}
task taskZ << {
    println 'taskZ'
}
taskX.dependsOn taskY
taskY.dependsOn taskZ
taskZ.shouldRunAfter taskX
```

**gradle -q taskX** 的输出

```
> gradle -q taskX
taskZ
taskY
taskX
```

