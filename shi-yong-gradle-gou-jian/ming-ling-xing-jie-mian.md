# 命令行界面

直接使用命令行界面是使用 Gradle 的主要方法之一. 接下来将学习如何使用命令行执行和自定义 Gradle.

强烈建议使用 [Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html). 你应该使用`gradle` 来代替`./gradlew` 和 `gradlew.bat`.

执行 Gradle 的命令行遵循以下结构. Options 既可以添加在任务名之前也可以在任务名之后:

```
gradle [taskName...] [--option-name...]
```

如果需要执行多个任务, 需要在任务名称之间添加空格.

部分需要赋值的 Options 不是强制需要在它和对应的值之间添加`=`; 但是, 还是强烈推荐使用`=`.

```
--console=plain
```

部分启动某些行为的 Options, 它的相反的选项就是在之前添加`--no-`. 比如:

```
--build-cache
--no-build-cache
```

许多长的 option 都有缩写形式, 比如:

```
--help
-h
```

许多命令行标志都可以在`gradle.properties`中直接指定. 可以查看 [configuring build environment guide](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties) 了解更多.

