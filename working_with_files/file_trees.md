# 文件树

文件树就是一个按照层次结构分布的文件集合,例如,一个文件树可以代表一个目录树结构或者一个 ZIP 压缩文件的内容.它被抽象为 `FileTree` 结构,`FileTree` 继承自 `FileCollection`,所以你可以像处理文件集合一样处理文件树, Gradle 有些对象实现了`FileTree` 接口,例如  [源集合](https://docs.gradle.org/current/userguide/java_plugin.html#sec:source_sets).
使用 `Project.fileTree()` 方法可以得到 `FileTree` 的实例,它会创建一个基于基准目录的对象,然后视需要使用一些 `Ant-style` 的包含和去除规则.

**例 15.5 创建文件树**
**build.gradle**

```
/以一个基准目录创建一个文件树
FileTree tree = fileTree(dir: 'src/main')

// 添加包含和排除规则
tree.include '**/*.java'
tree.exclude '**/Abstract*'

// 使用路径创建一个树
tree = fileTree('src').include('**/*.java')

// 使用闭合创建一个数
tree = fileTree('src') {
    include '**/*.java'
}

// 使用map创建一个树
tree = fileTree(dir: 'src', include: '**/*.java')
tree = fileTree(dir: 'src', includes: ['**/*.java', '**/*.xml'])
tree = fileTree(dir: 'src', include: '**/*.java', exclude: '**/*test*/**')

```

就像使用文件集合一样,你可以访问文件树的内容,使用 `Ant-style` 规则选择一个子树。

**例 15.6 使用文件树**
**build.gradle**

```
// 遍历文件树
tree.each {File file ->
    println file
}

// 过滤文件树
FileTree filtered = tree.matching {
    include 'org/gradle/api/**'
}

// 合并文件树A
FileTree sum = tree + fileTree(dir: 'src/test')

// 访问文件数的元素
tree.visit {element ->
    println "$element.relativePath => $element.file"
}

```











