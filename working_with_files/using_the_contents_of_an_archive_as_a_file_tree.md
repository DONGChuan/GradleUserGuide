# Using the contents of an archive as a file tree

Many objects in Gradle have properties which accept a set of input files. For example, the JavaCompile task has a source property, which defines the source files to compile. You can set the value of this property using any of the types supported by the files() method, which was shown above. This means you can set the property using, for example, a File, String, collection, FileCollection or even a closure. Here are some examples:

在 `Gradle` 中一些对象的某些属性可以接收输入文件集合.例如,`JavaComplile` 任务有一个 `source` 属性,它定义了编译的源文件,你可以设置 `files()` 方法支持的任何类型的值. 这意味着你可以使用 `File`,`String`,`collection`,`FileCollection`
甚至是一个闭合设置属性的值.

**例子 15.8 指定文件**
**build.gradle**
```
//使用一个 File 对象设置源目录
compile {
    source = file('src/main/java')
}

//使用一个字符路径设置源目录
compile {
    source = 'src/main/java'
}

// 使用一个集合设置多个源目录
compile {
    source = ['src/main/java', '../shared/java']
}

// 使用 FileCollection 或者 FileTree 设置源目录
compile {
    source = fileTree(dir: 'src/main/java').matching { include 'org/gradle/api/**' }
}

// 使用一个闭合设置源目录
compile {
    source = {
        // Use the contents of each zip file in the src dir
        file('src').listFiles().findAll {it.name.endsWith('.zip')}.collect { zipTree(it) }
    }
}
```

Usually, there is a method with the same name as the property, which appends to the set of files. Again, this method accepts any of the types supported by the files() method.

通常情况下,会有一个命名和属性名相同的方法设置属性,这个 方法 接收 `files()` 方法支持的任何类型的值.

** 例子 15.9 指定文件**
**build.gradle**
```
compile {
    // 使用字符路径添加源目录
    source 'src/main/java', 'src/main/groovy'

    // 使用File对象添加源目录
    source file('../shared/java')

    // 使用闭合添加源目录
    source { file('src/test/').listFiles() }
}

```







