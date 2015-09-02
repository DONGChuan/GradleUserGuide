# 文件树
A file tree is a collection of files arranged in a hierarchy. For example, a file tree might represent a directory tree or the contents of a ZIP file. It is represented by the FileTree interface. The FileTree interface extends FileCollection, so you can treat a file tree exactly the same way as you would a file collection. Several objects in Gradle implement the FileTree interface, such as source sets.

One way to obtain a FileTree instance is to use the Project.fileTree() method. This creates a FileTree defined with a base directory, and optionally some Ant-style include and exclude patterns.

文件树就是一个按照层次结构分布的文件集合,例如,一个文件树可以代表一个目录树结构或者一个ZIP压缩文件的内容.它被抽象为`FileTree`结构,`FileTree`继承自`FileCollection`,所以你可以像处理普通文件集合一样处理文件树, Gradle 有些对象实现了`FileTree`接口,例如 [source](https://docs.gradle.org/current/userguide/java_plugin.html#sec:source_sets).
使用`Project.fileTree()`方法可以得到`FileTree`的实例,它会创建一个基于基准目录的对象,然后视需要使用一些`Ant-style`的包含和排除规则.

**例子 15.5 创建文件树**
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


You use a file tree in the same way you use a file collection. You can also visit the contents of the tree, and select a sub-tree using Ant-style patterns:

就像使用普通文件集合一样,你可以访问文件树的内容，使用`Ant-style`规则选择一个子树。

**例15.6 使用文件树**
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











