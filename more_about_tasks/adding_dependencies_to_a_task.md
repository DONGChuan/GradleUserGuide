# 给 task 加入依赖

有许多种加入依赖的方式. 在 6.5 小节, “任务依赖”里, 你已经学习了如何使用任务的名称定义依赖. 任务名称可以指向同一个项目里的任务, 或者其他项目里的任务. 为了指向其他项目, 你必须在任务的名称前加入项目的路径. 下面的例子给 projectA:taskX 加入依赖 projectB:taskY :

**例子 15.11. 从另外一个项目给任务加入依赖**

**build.gradle**

```
project('projectA') {
    task taskX(dependsOn: ':projectB:taskY') << {
        println 'taskX'
    }
}

project('projectB') {
    task taskY << {
        println 'taskY'
    }
}
```

**gradle -q taskX** 的输出

```
> gradle -q taskX
taskY
taskX
```

除了使用任务名称, 你也可以定义一个依赖对象y:

**例子 15.12. 通过任务对象加入依赖**

**build.gradle**

```
task taskX << {
    println 'taskX'
}

task taskY << {
    println 'taskY'
}

taskX.dependsOn taskY
```

**gradle -q taskX** 的输出

```
> gradle -q taskX
taskY
taskX
```

更加先进的用法, 你可以通过闭包定义一个任务依赖. 闭包只能返回一个单独的 Task 或者 Task 对象的 collection, 这些返回的任务就将被当做依赖. 接下来的例子给 taskX 加入了一个复杂的依赖, 所有以 lib 开头的任务都将在 taskX 之前执行:

**例子 15.13. 通过闭包加入依赖**

**build.gradle**
```

task taskX << {
    println 'taskX'
}

taskX.dependsOn {
    tasks.findAll { task -> task.name.startsWith('lib') }
}

task lib1 << {
    println 'lib1'
}

task lib2 << {
    println 'lib2'
}

task notALib << {
    println 'notALib'
}
```

**gradle -q taskX** 的输出

```
> gradle -q taskX
lib1
lib2
taskX
```


For more information about task dependencies, see the Task API.
