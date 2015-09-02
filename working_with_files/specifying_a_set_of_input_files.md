# 指定一组输入文件


在 `Gradle` 中有一些对象的某些属性可以接收一组输入文件.例如,`JavaComplile` 任务有一个 `source` 属性,它定义了编译的源文件,你可以设置这个属性的值,只要 `files()` 方法支持. 这意味着你可以使用 `File` , `String` , `collection` , `FileCollection` 
甚至是使用一个闭合去设置属性的值.

**例 15.8 指定文件**

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

通常情况下,会有一个方法名和属性名相同的方法能够附加一组文件,这个方法接收 `files()` 方法支持的任何类型的值.

** 例 15.9 指定文件**

**build.gradle**
```
compile {
    // 使用字符路径添加源目录
    source 'src/main/java', 'src/main/groovy'

    // 使用 File 对象添加源目录
    source file('../shared/java')

    // 使用闭合添加源目录
    source { file('src/test/').listFiles() }
}

```
