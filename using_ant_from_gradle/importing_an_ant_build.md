## 导入一个Ant构建

你可以在你的gradle项目中通过`ant.importBuild()`来导入一个ant构建,当你导入了一个ant构建,每一个`ant target`都会被视为一个Gradle任务.这意味着你可以像操作,执行gradle任务一样操作,执行`ant target`

**例 16.8.导入ant构建**

**build.gradle**
```gradle
ant.importBuild 'build.xml'
```
**build.xml**
```xml
<project>
    <target name="hello">
        <echo>Hello, from Ant</echo>
    </target>
</project>
```

`gradle hello`的输出
>\> gradle hello
>:hello
>[ant:echo] Hello, from Ant
>
>BUILD SUCCESSFUL
>
>Total time: 1 secs


你可以添加一个依赖于`ant target`的任务:
**例 16.9.依赖于ant target的任务**

**build.gradle**
```gradle
ant.importBuild 'build.xml'

task intro(dependsOn: hello) << {
    println 'Hello, from Gradle'
}
```

>`gradle intro`的输出
>\> gradle intro
>:hello
>[ant:echo] Hello, from Ant
>:intro
>Hello, from Gradle
>
>BUILD SUCCESSFUL
>
>Total time: 1 secs

或者,你可以为`ant target`添加动作

**例 16.10.为Ant target添加动作**

**build.gradle**
```gradle
ant.importBuild 'build.xml'

hello << {
    println 'Hello, from Gradle'
}
```

`gradle hello`的输出
>\> gradle hello
>:hello
>[ant:echo] Hello, from Ant
>Hello, from Gradle
>
>BUILD SUCCESSFUL
>
>Total time: 1 secs

当然,一个`ant target`也可以依赖于gradle的任务

**例 16.11.为Ant target添加动作**

**build.gradle**
```gradle
ant.importBuild 'build.xml'

task intro << {
    println 'Hello, from Gradle'
}
```
**build.xml**
```xml
<project>
    <target name="hello" depends="intro">
        <echo>Hello, from Ant</echo>
    </target>
</project>
```

`gradle hello`的输出
>\> gradle hello
>:intro
>Hello, from Gradle
>:hello
>[ant:echo] Hello, from Ant
>
>BUILD SUCCESSFUL
>
>Total time: 1 secs

有时候可能需要'重命名'`ant target`以避免与现有的gradle任务冲突.需要使用[AntBuilder.importBuilder()](https://docs.gradle.org/current/javadoc/org/gradle/api/AntBuilder.html#importBuild(java.lang.Object, org.gradle.api.Transformer))方法.

**例 16.12.重命名导入的ant target**

**build.gradle**
```gradle
ant.importBuild('build.xml') { antTargetName ->
    'a-' + antTargetName
}
```
**build.xml**
```xml
<project>
    <target name="hello">
        <echo>Hello, from Ant</echo>
    </target>
</project>
```

`gradle a-hello`的输出
>\> gradle a-hello
>:a-hello
>[ant:echo] Hello, from Ant
>
>BUILD SUCCESSFUL
>
>Total time: 1 secs

注意,方法的二个参数应该是一个[TransFormer](https://docs.gradle.org/current/javadoc/org/gradle/api/Transformer.html),在Groovy编程的时候，由于[Groovy的支持自动闭包单抽象方法的类型](http://mrhaki.blogspot.ie/2013/11/groovy-goodness-implicit-closure.html)。我们可以简单地使用闭包取代匿名内部类，

