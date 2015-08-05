# File collections
A file collection is simply a set of files. It is represented by the FileCollection interface. Many objects in the Gradle API implement this interface. For example, dependency configurations implement FileCollection.

One way to obtain a FileCollection instance is to use the Project.files() method. You can pass this method any number of objects, which are then converted into a set of File objects. The files() method accepts any type of object as its parameters. These are evaluated relative to the project directory, as per the file() method, described in Section 15.1, “Locating files”. You can also pass collections, iterables, maps and arrays to the files() method. These are flattened and the contents converted to File instances.

 `file collection`表示一组文件，Gradle 中使用 `FileCollection` 接口表示,Gradle API中的许多项目都实现了这个接口,例如 [dependency configurations ](https://docs.gradle.org/current/userguide/dependency_management.html#sub:configurations).

 获取`FileCollection`实例的一种方法是使用`Project.files()`方法.你可以传递任何数量的对象参数,这个方法能将你传递的对象集合转换为一组文件对象.`files()`方法接收任何类型对象参数.每一个`file()`方法都依赖于项目目录(在第15章,第一小节中介绍).`files()`方法也接收 collections , iterables, maps 和 arrays 类型参数.这些参数的内容会被解析，然后被转换为文件对象.

**例子 15.2 创建文件集合 **
**build.gradle**
```
FileCollection collection = files('src/file1.txt',
                                  new File('src/file2.txt'),
                                  ['src/file3.txt', 'src/file4.txt'])
```








