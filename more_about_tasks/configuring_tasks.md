# 配置 tasks

举一个例子, 让我们看一看 Gradle 自带的 Copy task. 为了创建一个 Copy task, 你需要在你的构建脚本里先声明它:

**例子 15.7. 创建一个 copy task**

**build.gradle**

```
task myCopy(type: Copy)
```

它创建了一个没有默认行为的 copy task. 这个 task 可以通过它的 API 来配置(参考 [Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html)). 接下来例子展示了不同的实现方法.

补充说明一下, 这个 task 的名字是 “myCopy”, 但是它是 “Copy” 类(type). 你可以有许多同样 type 不同名字的 tasks. 这个在实现特定类型的所有任务的 cross-cutting concerns 时特别有用.

**例子 15.8. 配置一个任务 - 不同的方法**

**build.gradle**

```
Copy myCopy = task(myCopy, type: Copy)
myCopy.from 'resources'
myCopy.into 'target'
myCopy.include('**/*.txt', '**/*.xml', '**/*.properties')
```

这个我们通过 Java 配置对象是一样的形式. 但是你每次都必须在语句里重复上下文 (myCopy). 这种方式可能读起来并不是那么的漂亮.

下面一种方式就解决了这个问题. 是公认的最具可读性的方式.

**例子 15.9. 配置一个任务 - 通过闭包 closure**

**build.gradle**

```
task myCopy(type: Copy)

myCopy {
   from 'resources'
   into 'target'
   include('**/*.txt', '**/*.xml', '**/*.properties')
}
```

上面例子里的第三行是 tasks.getByName() 方法的一个简洁的写法. 特别要注意的是, 如果你通过闭包的形式来实现 getByName() 方法, 这个闭包会在 task 配置的时候执行而不是在 task 运行的时候执行.

你也可以直接在定义 task 时使用闭包.

**例子 15.10. 通过定义一个任务**

```
build.gradle

task copy(type: Copy) {
   from 'resources'
   into 'target'
   include('**/*.txt', '**/*.xml', '**/*.properties')
}
```

请不要忘了构建的各个阶段.

一个任务有配置和动作. 当使用 << 时, 你只是简单的使用捷径定义了动作. 定义在配置区域的代码只会在构建的配置阶段执行, 而且不论执行哪个任务. 可以参考第 55 章, The Build Lifecycle for more details about the build lifecycle.

