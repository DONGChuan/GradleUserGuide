# 使用插件的插件DSL

> 插件DSL正在孵化([incubating](https://docs.gradle.org/current/userguide/feature_lifecycle.html))中,请注意,在以后的Gradle版本中,DSL和其它配置可能会改变.

新的插件DSL提供了更为简洁,方便的方式来声明插件的依赖关系。它的适用于与新的[Gradle Plugin Portal](http://plugins.gradle.org/)，同时提供了方便的核心和社区插件.该插件脚本块配置[PluginDependenciesSpec](https://docs.gradle.org/current/dsl/org.gradle.plugin.use.PluginDependenciesSpec.html)的实例.

要应用的核心插件,可以使用短名称:

**Example 21.5. Applying a core plugin**

**build.gradle**

```gradle
plugins {
    id 'java'
}
```

要从插件门户应用一个社区插件,必须使用插件的完全限定id:

**Example 21.6. Applying a community plugin**

**build.gradle**

```gradle
plugins {
    id "com.jfrog.bintray" version "0.4.1"
}
```

不必要进行进一步的配置,就是说没有必要配置buildscript的类路径,Gradle会从插件门户找到该插件,并使构建可用.

参见[PluginDependenciesSpec](https://docs.gradle.org/current/dsl/org.gradle.plugin.use.PluginDependenciesSpec.html)查看关于使用插件DSL的更多信息。
