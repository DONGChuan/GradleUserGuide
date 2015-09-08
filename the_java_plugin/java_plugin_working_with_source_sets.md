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

To configure an existing source set, you simply use one of the above access methods to set the properties of the source set. The properties are described below. Here is an example which configures the main Java and resources directories:

Example 22.4. Configuring the source directories of a source set

build.gradle
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