## 在构建中使用自定义Ant任务

为了让你的构建可以自定义任务,你可以使用`taskdef(通常更容易)`或者`typedef`Ant任务,就像你在一个`build.xml`文件中一样.然后，您可以参考内置Ant任务去定制Ant任务.

**例 16.5.使用自定义Ant任务**

**build.gradle**
```gradle
task check << {
    ant.taskdef(resource: 'checkstyletask.properties') {
        classpath {
            fileset(dir: 'libs', includes: '*.jar')
        }
    }
    ant.checkstyle(config: 'checkstyle.xml') {
        fileset(dir: 'src')
    }
}
```

你可以使用Gradle的依赖管理去组装你自定义任务所需要的classpath.要做到这一点，你需要定义一个自定义配置类路径，然后添加一些依赖配置.在[Section 51.4, “How to declare your dependencies”](https://docs.gradle.org/current/userguide/dependency_management.html#sec:how_to_declare_your_dependencies)部分有更详细的说明.

**例 16.6.声明类路径的自定义Ant任务**

**build.gradle**
```gradle
configurations {
    pmd
}

dependencies {
    pmd group: 'pmd', name: 'pmd', version: '4.2.5'
}
```

要使用classpath配置，使用自定义配置的`asPath`属性。

**例 16.7.一起使用自定义Ant任务和依赖管理**

**build.gradle**

```gradle
task check << {
    ant.taskdef(name: 'pmd',
                classname: 'net.sourceforge.pmd.ant.PMDTask',
                classpath: configurations.pmd.asPath)
    ant.pmd(shortFilenames: 'true',
            failonruleviolation: 'true',
            rulesetfiles: file('pmd-rules.xml').toURI().toString()) {
        formatter(type: 'text', toConsole: 'true')
        fileset(dir: 'src')
    }
}
```
