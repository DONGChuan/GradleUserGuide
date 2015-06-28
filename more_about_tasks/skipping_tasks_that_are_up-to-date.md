# 跳过 up-to-date 的任务

如果你正在使用一些附加的任务, 比如通过 Java 插件加入的任务, 你可能会注意到 Gradle 会跳过一些任务, 这些任务后面会标注 **up-to-date**. 代表这个任务已经运行过了或者说是最新的状态, 不再需要产生一次相同的输出.  不仅仅是这些内建任务, 其实你在运行自己的任务时, 也会碰到这种情况.

### 1. 声明一个任务的输入和输出

让我们先看一个例子. 这里我们的任务会根据一个 XML 文件生成好几个输出文件. 让我们运行这个任务 2 次.

**例子 15.23. A generator task**

**build.gradle**

```
task transform {
    ext.srcFile = file('mountains.xml')
    ext.destDir = new File(buildDir, 'generated')
    doLast {
        println "Transforming source file."
        destDir.mkdirs()
        def mountains = new XmlParser().parse(srcFile)
        mountains.mountain.each { mountain ->
            def name = mountain.name[0].text()
            def height = mountain.height[0].text()
            def destFile = new File(destDir, "${name}.txt")
            destFile.text = "$name -> ${height}\n"
        }
    }
}
```

**gradle transform** 的输出

```
> gradle transform
:transform
Transforming source file.
```

**gradle transform** 的输出

```
> gradle transform
:transform
Transforming source file.
```

这里 Gradle 执行了这个任务两次, 即使什么都没有改变,  它也没有跳过这个任务. 这个例子里的任务, 它的行为是通过闭包定义的. Gradle 并不知道闭包会做什么, 也并不能自动指出是否这个任务是 up-to-date. 为了使用 Gradle 的 up-to-date 检测, 你需要**定义任务的输入和输出**.

每个任务都有输入和输出属性, 你需要使用这些属性来声明任务的输入和输出. 下面的例子中, 我们将声明 XML 文件作为输入, 并且把输出放在一个指定的目录. 让我们运行这个任务 2 次.

**例子 15.24. 声明任务的输入和输出**

**build.gradle**

```
task transform {
    ext.srcFile = file('mountains.xml')
    ext.destDir = new File(buildDir, 'generated')
    inputs.file srcFile
    outputs.dir destDir
    doLast {
        println "Transforming source file."
        destDir.mkdirs()
        def mountains = new XmlParser().parse(srcFile)
        mountains.mountain.each { mountain ->
            def name = mountain.name[0].text()
            def height = mountain.height[0].text()
            def destFile = new File(destDir, "${name}.txt")
            destFile.text = "$name -> ${height}\n"
        }
    }
}
```

**gradle transform** 的输出
```
> gradle transform
:transform
Transforming source file.
```
**gradle transform** 的输出
```
> gradle transform
:transform UP-TO-DATE
```

现在, Gradle 就能够检测出任务是否是 up-to-date 状态.

任务的输入属性是 [TaskInputs 类型](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/TaskInputs.html). 任务的输出属性是 [TaskOutputs 类型](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/TaskOutputs.html).

一个任务如果没有定义输出的话, 那么它永远都没用办法判断 up-to-date. 对于某些场景, 比如一个任务的输出不是文件, 或者更复杂的场景, [TaskOutputs.upToDateWhen()](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/TaskOutputs.html#upToDateWhen(groovy.lang.Closure)) 方法会计算任务的输出是否应被视为最新.

总而言之, **如果一个任务只定义了输出, 如果输出不变的话, 它就会被视为 up-to-date.**

### 2. 它是如何工作的?

当一个任务是首次执行时, Gradle 会取一个输入的快照 (snapshot). 该快照包含组输入文件和每个文件的内容的散列. 然后当 Gradle 执行任务时, 如果任务成功完成，Gradle 会获得一个输出的快照. 该快照包含输出文件和每个文件的内容的散列. Gradle 会保留这两个快照用来在该任务的下一次执行时进行判断.

之后, 每次在任务执行之前, Gradle 都会为输入和输出取一个新的快照, 如果这个快照和之前的快照一样, Gradle 就会假定这个任务已经是最新的 (up-to-date) 并且跳过任务, 反之亦然.

需要注意的是, 如果一个任务有指定的输出目录, 自从该任务上次执行以来被加入到该目录的任务文件都会被忽略, 并且不会引起任务过时 (out of date). 这是因为不相关任务也许会共用同一个输出目录. 如果这并不是你所想要的情况, 可以考虑使用 [TaskOutputs.upToDateWhen()](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/TaskOutputs.html#upToDateWhen(groovy.lang.Closure))


