# 命令行界面

直接使用命令行界面是使用 Gradle 的主要方法之一. The following serves as a reference of executing and customizing Gradle use of a command-line or when writing scripts or configuring continuous integration.

Use of the[Gradle Wrapper](https://docs.gradle.org/current/userguide/gradle_wrapper.html)is highly encouraged. You should substitute`./gradlew`or`gradlew.bat`for`gradle`in all following examples when using the Wrapper.

执行 Gradle 的命令行遵循以下结构. Options 既可以添加在任务名之前也可以在任务名之后:

```
gradle [taskName...] [--option-name...]
```

If multiple tasks are specified, they should be separated with a space.

Options that accept values can be specified with or without`=`between the option and argument; however, use of`=`is recommended.

```
--console=plain
```

Options that enable behavior have long-form options with inverses specified with`--no-`. The following are opposites.

```
--build-cache
--no-build-cache
```

许多长的 option 都有缩写形式, 比如:

```
--help
-h
```

Many command-line flags can be specified in`gradle.properties`to avoid needing to be typed. See the[configuring build environment guide](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_configuration_properties)for details.

The following sections describe use of the Gradle command-line interface, grouped roughly by user goal. Some plugins also add their own command line options, for example[`--tests`for Java test filtering](https://docs.gradle.org/current/userguide/java_plugin.html#test_filtering). For more information on exposing command line options for your own tasks, see[the section called “Declaring and Using Command Line Options”](https://docs.gradle.org/current/userguide/custom_tasks.html#sec:declaring_and_using_command_line_options).

