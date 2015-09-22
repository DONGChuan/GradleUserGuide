# 定位 tasks

你经常需要在构建文件里找到你定义的 tasks, 
举个例子, 
为了配置它们或者使用它们作为依赖. 有许多种方式都可以来实现定位. 
首先, 
每一个任务都必须是一个 project 的有效属性, 并使用任务名来作为属性名:

**例子 15.4. 通过属性获取 tasks**

**build.gradle**
```
task hello

println hello.name
println project.hello.name
```

Tasks 也可以通过 tasks collection 来得到.

**例子 15.5. 通过 tasks collection 获取 tasks**

**build.gradle**

```
task hello

println tasks.hello.name
println tasks['hello'].name
```

You can access tasks from any project using the task's path using the tasks.getByPath() method. You can call the getByPath() method with a task name, or a relative path, or an absolute path.

**例子 15.6. 通过路径获取 tasks**

**build.gradle**

```
project(':projectA') {
    task hello
}

task hello

println tasks.getByPath('hello').path
println tasks.getByPath(':hello').path
println tasks.getByPath('projectA:hello').path
println tasks.getByPath(':projectA:hello').path
```

**gradle -q hello** 的输出

```
> gradle -q hello
:hello
:hello
:projectA:hello
:projectA:hello
```
参考
[TaskContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/TaskContainer.html) 可以知道跟多关于定位 tasks 的选项.


