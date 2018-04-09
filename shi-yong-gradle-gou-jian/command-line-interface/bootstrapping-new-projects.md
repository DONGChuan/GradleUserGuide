# 创建新项目

### 创建新的 Gradle 构建

可以在一个全新的或者已经存在的项目里使用内建的 `gradle init` 任务来创建一个新的 Gradle 构建.

```
❯ gradle init
```

许多时候, 你可能想要指定项目的类型. 一般有`basic`\(默认的\),`java-library`,`java-application` 等等. 阅读[初始化插件](https://docs.gradle.org/4.6/userguide/build_init_plugin.html)了解更多.

```
❯ gradle init --type java-library
```

### Standardize and provision Gradle

内建的 `gradle wrapper` 任务会生成一个脚本,`gradlew`, 将调用声明的 Gradle 版本, 如果需要的话, 会提前下载 Gradle.

```
❯ gradle wrapper --gradle-version=4.4
```

你可以使用 `--distribution-type=(bin|all)`,`--gradle-distribution-url`,`--gradle-distribution-sha256-sum`包括`--gradle-version` 来进行一些特殊的指定. 具体可以查看[Gradle Wrapper](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html).

