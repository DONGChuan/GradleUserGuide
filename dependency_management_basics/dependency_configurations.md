# 依赖配置

在 Gradle 里, 依赖可以组合成*configurations(配置)*. 一个配置简单地说就是一系列的依赖. 我们称它们为*(dependency configuration)依赖配置*. 你可以使用它们声明项目的外部依赖. 正如我们将在后面看到, 它们也被用来声明项目的发布.

Java 插件定义了许多标准的配置. 下面列出了一些, 你也可以在[Table 23.5, “Java 插件 - 依赖管理”](https://dongchuan.gitbooks.io/gradle-user-guide-/content/the_java_plugin/java_plugin_dependency_management.html)里发现更多具体的信息.

**compile**

用来编译项目源代码的依赖.

**runtime**

在运行时被生成的类使用的依赖. 默认的, 也包含了编译时的依赖.

**testCompile**

编译测试代码的依赖. 默认的, 包含生成的类运行所需的依赖和编译源代码的依赖.

**testRuntime**

运行测试所需要的依赖. 默认的, 包含上面三个依赖.

各种各样的插件加入许多标准的配置. 你也可以定义你自己的配置. 参考 [Section 52.3, “配置依赖”](https://docs.gradle.org/current/userguide/dependency_management.html#sub:configurations)可以找到更加具体的定义和定制一个自己的依赖配置.

