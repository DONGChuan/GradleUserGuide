## 16.1.使用Ant任务和类型的构建

在构建脚本中，ant属性是由Gradle提供的.这是一个参考的[AntBuilder](https://docs.gradle.org/current/javadoc/org/gradle/api/AntBuilder.html)实例.AntBuilder用于从构建脚本访问Ant任务，类型和属性。下面的例子解释了从Ant的`build.xml`格式到Grooy的映射.

你可以通过调用AntBuilder实例的方法执行Ant任务.您可以使用任务名称作为方法名,比如,可以通过调用`anto.echo()`任务执行Ant`echo`任务.Ant任务属性通过Map参数传递给方法.下面是一个`echo`任务的例子,注意我们也可以混合使用Groovy代码和Ant任务标记,这点非常强大.

**例 16.1.使用Ant任务**

**build.gradle**

```gradle
task hello << {
    String greeting = 'hello from Ant'
    ant.echo(message: greeting)
}
```

`gradle hello`的输出

>\> gradle hello
>:hello
>[ant:echo] hello from Ant
>
>BUILD SUCCESSFUL
>
>Total time: 1 secs

你可以添加嵌套元素添加到一个封闭的Ant任务的内部.定义嵌套元素跟定义任务的方式相同,通过与调用我们要定义的元素名相同的方法.

**例 16.3.添加嵌套元素到一个Ant任务**

**build.gradle**
```gradle
task zip << {
    ant.zip(destfile: 'archive.zip') {
        fileset(dir: 'src') {
            include(name: '**.xml')
            exclude(name: '**.java')
        }
    }
}
```

你可以像访问任务一样访问ant type,只需要将type名作为方法名即可.该方法调用返回的ant数据类型，可以直接在构建脚本中使用.下面的例子中,我们创建了一个`ant path`对象,然后遍历他的内容.

**例 16.4.使用ant类型**

**build.gradle**
```gradle
task list << {
    def path = ant.path {
        fileset(dir: 'libs', includes: '*.jar')
    }
    path.list().each {
        println it
    }
}
```

更多关于AntBuilder的信息可以参考'Groovy in Action'8.4或者[Groovy Wiki](http://groovy.codehaus.org/Using+Ant+from+Groovy).

