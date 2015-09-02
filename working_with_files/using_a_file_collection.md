# Using a file collection
A file collection is iterable, and can be converted to a number of other types using the as operator. You can also add 2 file collections together using the + operator, or subtract one file collection from another using the - operator. Here are some examples of what you can do with a file collection.

文件集合可以被迭代器,使用迭代操作能够将其转换为其他的一些类型.你可以使用`+`将两个文件集合合并,使用`-`能够对一个文件集合做减法.下面一些例子介绍如何操作文件集合.
**例子 15.3 使用文件集合**

***build.gradle**

```
// 对文件集合进行迭代
collection.each {File file ->
    println file.name
}

// 转换文件集合为其他类型
Set set = collection.files
Set set2 = collection as Set
List list = collection as List
String path = collection.asPath
File file = collection.singleFile
File file2 = collection as File

// 增加和减少文件集合
def union = collection + files('src/file3.txt')
def different = collection - files('src/file3.txt')

```

You can also pass the files() method a closure or a Callable instance. This is called when the contents of the collection are queried, and its return value is converted to a set of File instances. The return value can be an object of any of the types supported by the files() method. This is a simple way to 'implement' the FileCollection interface.

你也可以向 `files()` 方法专递一个闭合或者可回调的实例参数.当查询集合的内容时就会调用它,然后将返回值转换为一些文件实例.返回值可以是`files()`方法支持的任何类型的对象.下面有个简单的例子来演示实现`FileCollection`接口

**例子15.4 实现一个文件集合**
**build.gradle**
```
task list << {
    File srcDir

    // 使用闭合创建一个文件集合
    collection = files { srcDir.listFiles() }

    srcDir = file('src')
    println "Contents of $srcDir.name"
    collection.collect { relativePath(it) }.sort().each { println it }

    srcDir = file('src2')
    println "Contents of $srcDir.name"
    collection.collect { relativePath(it) }.sort().each { println it }
}

```
使用`gradle -q list`输出结果

```
> gradle -q list
Contents of src
src/dir1
src/file1.txt
Contents of src2
src2/dir1
src2/dir2

```

`files()`方法也接收其他类型的参数:

**FileCollection**
These are flattened and the contents included in the file collection.
**Task**
The output files of the TaskOutputs are included in the file collection.

**TaskOutputs**
The output files of the TaskOutputs are included in the file collection.

It is important to note that the content of a file collection is evaluated lazily, when it is needed. This means you can, for example, create a FileCollection that represents files which will be created in the future by, say, some task.

值得注意的是文件集合的内容是被惰性,当你需要的时候，就比如创建一个`FileCollecion`代表文件集合会被一些`task`在需要的时候被创建.
















