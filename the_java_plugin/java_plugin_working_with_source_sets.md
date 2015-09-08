# 使用资源设置

你可以通过 sourceSets 属性来使用资源设置. 它其实是一个项目资源设置的容器, 它的类型是 [SourceSetContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/SourceSetContainer.html). 同时也有 sourceSets { } 脚本模块, 在这个模块里, 你可以通过闭包配置资源设置容器. 资源设置容器的工作方式和其余容器几乎一模一样, 比如 tasks.

**例 22.3. 进入资源设置**

**build.gradle**

```
// Various ways to access the main source set
println sourceSets.main.output.classesDir
println sourceSets['main'].output.classesDir
sourceSets {
    println main.output.classesDir
}
sourceSets {
    main {
        println output.classesDir
    }
}

// Iterate over the source sets
sourceSets.all {
    println name
}
```

为了配置一个已经存在的资源集, 你只需要使用上面提及的访问方法来设置资源集的属性. 下面就是一个配置 Java 资源目录的例子:

**例 22.4. 配置资源集的源目录**

**build.gradle**
```
sourceSets {
    main {
        java {
            srcDir 'src/java'
        }
        resources {
            srcDir 'src/resources'
        }
    }
}
```