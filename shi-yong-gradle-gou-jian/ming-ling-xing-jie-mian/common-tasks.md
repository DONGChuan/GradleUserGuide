# 常用 Tasks

Gradle 本身以及绝大多数 Gradle 插件都包含下列任务, 原文中称这是一种任务约定(task conventions).

### 计算所有的输出

在 Gradle 构建中, `build` 任务被用来集合所有的输出以及运行所有的检查.

```
> gradle build
```

### 运行应用

`run` 任务用来聚合应用, 执行脚本.

```
> gradle run
```

### 运行所有的检查

`check` 任务用来执行所有的检查任务, 包括测试和语法检查.

```
> gradle check
```

### 清除输出

You can delete the contents of the build directory using the`clean` 任务会删除构建目录里的内容, 之前的预计算输出都将丢失, 随后的任务执行时间都将大大加长.

```
> gradle clean
```
