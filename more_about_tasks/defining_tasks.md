# 定义 tasks

我们已经在第 6 章学习了定义任务的形式 (keyword 形式). 当然也会有一些定义形式的变化来适应某些特殊的情况. 比如下面的例子中任务名被括号括起来了. 这是因为之前定义简单任务的形式 (keyword 形式) 在表达式里是不起作用的.

**例子 15.1. 定义 tasks**

**build.gradle**
```
task(hello) << {
    println "hello"
}

task(copy, type: Copy) {
    from(file('srcDir'))
    into(buildDir)
}
```
你也可以使用 strings 来定义任务的名字:

**例子 15.2. 例子 tasks - 使用 strings 来定义任务的名字**

**build.gradle**
```
task('hello') <<
{
    println "hello"
}

task('copy', type: Copy) {
    from(file('srcDir'))
    into(buildDir)
}
```

还有另外一种语法形式来定义任务, 更加直观:

**例子 15.3. 另外一种语法形式**

**build.gradle**

```
tasks.create(name: 'hello') << {
    println "hello"
}

tasks.create(name: 'copy', type: Copy) {
    from(file('srcDir'))
    into(buildDir)
}
```

这里实际上我们把任务加入到 tasks collection 中. 可以看一看 [TaskContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/TaskContainer.html) 来深入了解下.

