#复制文件
You can use the Copy task to copy files. The copy task is very flexible, and allows you to, for example, filter the contents of the files as they are copied, and map to the file names.

To use the Copy task, you must provide a set of source files to copy, and a destination directory to copy the files to. You may also specify how to transform the files as they are copied. You do all this using a copy spec. A copy spec is represented by the CopySpec interface. The Copy task implements this interface. You specify the source files using the CopySpec.from() method. To specify the destination directory, use the CopySpec.into() method.

你可以使用 `Copy` 任务去复制文件. 复制任务扩展性很强,能够过滤所要复制文件的内容, 映射文件名.

使用 `Copy` 任务时需要提供想要复制的源文件和一个目标目录,如果你要指定的文件被复制时如何转换文件，可以使用 _复制规则_. 复制规则被 `CopySpec` 接口抽象,`Copy` 任务实现了这个接口. 使用 `CopySpec.from()` 方法指定源文件.使用 `CopySpec.into()` 方法指定目标目录.

**例 15.10. 使用复制任务复制文件**
**build.gradle**
```
task copyTask(type: Copy) {
    from 'src/main/webapp'
    into 'build/explodedWar'
}

```
The from() method accepts any of the arguments that the files() method does. When an argument resolves to a directory, everything under that directory (but not the directory itself) is recursively copied into the destination directory. When an argument resolves to a file, that file is copied into the destination directory. When an argument resolves to a non-existing file, that argument is ignored. If the argument is a task, the output files (i.e. the files the task creates) of the task are copied and the task is automatically added as a dependency of the Copy task. The into() accepts any of the arguments that the file() method does. Here is another example:

`from()` 方法接收任何 `files()` 方法支持的参数. 当参数代表一个目录时,在这个目录下的任何文件都会被复制到目标目录.当参数代表一个不存在的文件时,这个参数就会忽略.如果这个参数是一个任务，任务输出文件(这个任务创建的文件)会被复制,然后这个任务会被自动添加为 Copy 任务的依赖.

**Example 15.11. 指定复制任务的源文件和目标目录**
**build.gradle**
```
task anotherCopyTask(type: Copy) {
    // 复制 src/main/webapp 目录下的任何文件
    from 'src/main/webapp'
    // 单独复制一个文件
    from 'src/staging/index.html'
    // 复制一个任务输出的文件
    from copyTask
    // 显式使用outputs文件复制任务的输出
    from copyTaskWithPatterns.outputs
    // 复制一个 ZIP 压缩文件的内容
    from zipTree('src/main/assets.zip')
    // 最后指定目标目录
    into { getDestDir() }
}

```
You can select the files to copy using Ant-style include or exclude patterns, or using a closure:

你可以选择性的使用`Ant-style` 规则复制文件,或者使用一个闭合.

**例 15.12 选择性的复制文件**
**build.gradle**
```
task copyTaskWithPatterns(type: Copy) {
    from 'src/main/webapp'
    into 'build/explodedWar'
    include '**/*.html'
    include '**/*.jsp'
    exclude { details -> details.file.name.endsWith('.html') &&
                         details.file.text.contains('staging') }
}
```

You can also use the Project.copy() method to copy files. It works the same way as the task with some major limitations though. First, the copy() is not incremental (see Section 14.9, “Skipping tasks that are up-to-date”).

你也可以使用 `Project.copy()` 方法复制文件,它的工作方式和复制任务的一些主要限制相似,但是首先该方法不是增量的,请参考 [第 14.9节 跳过 up-to-date的任务](https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:up_to_date_checks)》

**例 15.13 在没有 up-to-date 检查的情况下使用 copy() 方法复制文件**

**build.gradle**
```
task copyMethod << {
    copy {
        from 'src/main/webapp'
        into 'build/explodedWar'
        include '**/*.html'
        include '**/*.jsp'
    }
}
```

第二,当一个任务被用来作为复制源时,这个 `copy()` 方法不能够实现任务依赖,因为它是一个普通的方法不是一个任务.因此,如果你使用 `copy()`方法作为一个任务功能的一部分,为了得到这个证的结果,你需要显式的声明所有的输入和输出.

**例 15.14 在使用 up-to-date 检查的情况下使用 copy() 方法复制文件**

**build.gradle**

```
task copyMethodWithExplicitDependencies{

     // 对输入做 up-to-date 检查附加 copyTask作为依赖
    inputs.file copyTask
    outputs.dir 'some-dir' // up-to-date 检查输出
    doLast{
        copy {
            // 负责 copyTask 的输出
            from copyTask
            into 'some-dir'
        }
    }
}

```

It is preferable to use the Copy task wherever possible, as it supports incremental building and task dependency inference without any extra effort on your part. The copy() method can be used to copy files as part of a task's implementation. That is, the copy method is intended to be used by custom tasks (see Chapter 60, Writing Custom Task Classes) that need to copy files as part of their function. In such a scenario, the custom task should sufficiently declare the inputs/outputs relevant to the copy action.

建议尽可能的使用复制任务,因为它支持增量化的构建和任务依赖推理，而不需要去额外的费力处理这些.不过 `copy()` 方法可以用来作为复制文件任务实现的一部分.即改 方法被用来在需要复制文件的自定义任务使用,请参考 [第60章 编写自定义任务](https://docs.gradle.org/current/userguide/custom_tasks.html).在这样的场景中，自定义任务应该充分声明与复制操作相关的输入/输出。

##15.6.1 重命名文件

**例 15.15 在复制时重命名文件**

**build.gradle**

```
task rename(type: Copy) {
    from 'src/main/webapp'
    into 'build/explodedWar'
    // 使用一个闭合映射文件名
    rename { String fileName ->
        fileName.replace('-staging-', '')
    }
    // 使用正则表达式映射文件名
    rename '(.+)-staging-(.+)', '$1$2'
    rename(/(.+)-staging-(.+)/, '$1$2')
}

```

##15.6.2 过滤文件

**例 15.16 在复制时过滤文件**

**build.gradle**

```
import org.apache.tools.ant.filters.FixCrLfFilter
import org.apache.tools.ant.filters.ReplaceTokens

task filter(type: Copy) {
    from 'src/main/webapp'
    into 'build/explodedWar'
    // 在文件中替代属性标记
    expand(copyright: '2009', version: '2.3.1')
    expand(project.properties)
    // 使用 Ant 提供的过滤器
    filter(FixCrLfFilter)
    filter(ReplaceTokens, tokens: [copyright: '2009', version: '2.3.1'])
    // 用一个闭合来过滤每一行
    filter { String line ->
        "[$line]"
    }
    // 使用闭合来删除行
    filter { String line ->
        line.startsWith('-') ? null : line
    }
}

```

A “token” in a source file that both the “expand” and “filter” operations look for, is formatted like “@tokenName@” for a token named “tokenName”.

扩大和过滤器操作都会查看的 `token`,格式类似 `@tokenName@` 名称为 `tokenName` 的 `token`.

## 15.6.3  使用 CopySpec 类

Copy specs form a hierarchy. A copy spec inherits its destination path, include patterns, exclude patterns, copy actions, name mappings and filters.

复制规范来自于层次结构,一个复制规范继承其目标路径,包括模式,排除模式，复制操作,名称映射和过滤器.

**例 15.17. 嵌套复制规范**

**build.gradle**

```
task nestedSpecs(type: Copy) {
    into 'build/explodedWar'
    exclude '**/*staging*'
    from('src/dist') {
        include '**/*.html'
    }
    into('libs') {
        from configurations.runtime
    }
}


```









































