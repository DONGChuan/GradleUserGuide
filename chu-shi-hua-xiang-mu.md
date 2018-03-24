# 初始化项目

首先, 创建一个新的目录:

```
❯ mkdir basic-demo
❯ cd basic-demo
```

使用 Gradle `init` 命令生成一个简单的项目:

```
❯ gradle init
Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 3s
2 actionable tasks: 2 executed
```

通过这个命令, 生成了一个'空项目. 如果没有生成成功, 请确保 Gradle 已经正确安装并且`JAVA_HOME` 设置正确.

目录结构如下

```
.
├── build.gradle  

├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar  

│       └── gradle-wrapper.properties  

├── gradlew  

├── gradlew.bat  

└── settings.gradle
```

* build.gradle - 项目配置脚本, 配置当前项目的各项任务.
* gradle-wrapper.jar - [Gradle Wrapper](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html) 可执行 JAR 文件.
* gradle-wrapper.properties - Gradle Wrapper 配置相关的各项属性.
* gradlew - 基于 Unix 系统的 Gradle Wrapper 脚本.
* gradlew.bat - Windows 系统的 Gradle Wrapper 脚本.
* settings.gradle - 配置设置脚本, 配置哪些项目会加入到构建当中.

> `gradle init` 可以生成[许多种不同类型的项目](https://docs.gradle.org/4.6/userguide/build_init_plugin.html#sec:build_init_types), 并把 `pom.xml` 文件转化成 Gradle.



