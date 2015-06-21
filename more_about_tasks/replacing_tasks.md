# 替换 tasks

有时候你想要替换一个任务. 举个例子, 如果你想要互换一个通过 java 插件定义的任务和一个自定义的不同类型的任务:

例子 14.19. 覆写一个任务

**build.gradle**

```
task copy(type: Copy)

task copy(overwrite: true) << {
    println('I am the new one.')
}
```

**gradle -q copy** 的输出

```
> gradle -q copy
I am the new one.
```

这种方式将用你自己定义的任务替换一个 Copy 类型的任务, 因为它使用了同样的名字.
但你定义一个新的任务时, 你必须设置 overwrite 属性为 true. 否则的话 Gradle 会抛出一个异常, task with that name already exists.

