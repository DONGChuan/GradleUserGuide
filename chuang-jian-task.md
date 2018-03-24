# 创建 Task

Gradle 通过 Groovy 或者 Kotlin-based DSL 提供相关的 APIs 来创建和配置任务. 一个[项目](https://docs.gradle.org/4.6/dsl/org.gradle.api.Project.html)包含许多[任务](https://docs.gradle.org/4.6/dsl/org.gradle.api.Task.html), 每个人物都会执行一些基础的操作.

Gradle 本身也自带了一些任务, 你可以直接在自己的项目中直接使用. 比如, 用来复制文件的`Copy`任务. `Copy`任务十分常用 \(具体可以[查看文档](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.Copy.html)了解更多\), 这里, 我们将只展示最基本的用法. 让我们来执行下列操作:

1. 创建一个文件夹`src`.

2. 在文件夹中添加 `myfile.txt`.  在文件中随意添加内容, 比如`Hello, World!`.

3. 在 `build.gradle`中定义一个叫做`copy` 的任务, 它的类型为`Copy`\(注意首字母大写\), 会将 `src`文件夹中的内容复制到一个新的文件夹, 叫做`dest`. \(你不需要手动创建`dest`文件夹— copy 任务会帮你完成它\)

```
task copy(type: Copy, group: "Custom", description: "Copies sources to the dest directory") {
    from "src"
    into "dest"
}
```

这里,`group`和`description`可以是任何字段. 你甚至可以删除它们, 但是这样也会将它们从`tasks`报告中删除, 由于我们接下来还需要使用它们, 请先不要删除.

现在可以执行`copy`任务了:

```
❯ ./gradlew copy
:copy

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```

请检查现在在`dest`文件夹下是否存在一个 `myfile.txt`文件, 并且该文件与原文件内容相同.

