## 22.7.使用sourceSet工作
可以使用sourceSet属性进行sourceSet,这是一个[SourceSetContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/SourceSetContainer.html)类型的项目的sourceSet容器.还可以通过sourceSet{}脚本块来配置sourceSet容器,sourceSet容器的工作方式基本与其他容器比如task的工作方式相同.

**例22.3.访问sourceSet**

**build.gradle**

```
//通过不同方式访问source set
println sourceSets.main.output.classesDir
println sourceSets['main'].output.classesDir
sourceSets{
  println main.output.classesDir
}
sourceSets{
  main{
    println output.classesDir
  }
}
// 遍历sourceSets
sourceSets.all{
  println name
}
```

配置现有的source set,只需要用上述的一种方式访问并设置source set,source set的配置如下所示,下面的例子主要配置了Java的main与resources目录:

**例22.4.配置一个source Set的资源路径**

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
