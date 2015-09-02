#复制文件

你可以使用复制任务( `Copy` )去复制文件. 复制任务扩展性很强,能够过滤复制文件的内容, 映射文件名.

使用复制任务时需要提供想要复制的源文件和一个目标目录,如果你要指定文件被复制时的转换方式，可以使用 _复制规则_. 复制规则被 `CopySpec` 接口抽象,复制任务实现了这个接口. 使用 `CopySpec.from()` 方法指定源文件.使用 `CopySpec.into()` 方法指定目标目录.

**例 15.10. 使用复制任务复制文件**

**build.gradle**
```
task copyTask(type: Copy) {
    from 'src/main/webapp'
    into 'build/explodedWar'
}

```

`from()` 方法接收任何 `files()` 方法支持的参数. 当参数被解析为一个目录时,在这个目录下的任何文件都会被递归地复制到目标目录(但不是目录本身).当一个参数解析为一个文件时,该文件被复制到目标目录中.当参数被解析为一个不存在的文件时,这个参数就会忽略.如果这个参数是一个任务,任务的输出文件(这个任务创建的文件)会被复制,然后这个任务会被自动添加为复制任务的依赖.

**Example 15.11. 指定复制任务的源文件和目标目录**

**build.gradle**
```
task anotherCopyTask(type: Copy) {
    // 复制 src/main/webapp 目录下的所有文件
    from 'src/main/webapp'
    // 复制一个单独文件
    from 'src/staging/index.html'
    // 复制一个任务输出的文件
    from copyTask
    // 显式使用任务的 outputs 属性复制任务的输出文件
    from copyTaskWithPatterns.outputs
    // 复制一个 ZIP 压缩文件的内容
    from zipTree('src/main/assets.zip')
    // 最后指定目标目录
    into { getDestDir() }
}

```

你可以使用`Ant-style` 规则或者一个闭合选择要复制的文件.

**例 15.12 选择要复制文件**

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
你也可以使用 `Project.copy()` 方法复制文件,它的工作方式有一些限制,首先该方法不是增量的,请参考 [第 14.9节 跳过最新的任务](https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:up_to_date_checks).第二,当一个任务被用作复制源时(例如 from() 方法的参数), `copy()` 方法不能够实现任务依赖,因为它是一个普通的方法不是一个任务.因此,如果你使用 `copy()`方法作为一个任务的一部分功能,你需要显式的声明所有的输入和输出以确保获得正确的结果.

**例 15.13 不使用最新检查方式下用 copy() 方法复制文件**

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



**例 15.14 使用最新的检查方式下用 copy() 方法复制文件**

**build.gradle**

```
task copyMethodWithExplicitDependencies{

     // 对输入做最新检查,添加 copyTask 作为依赖
    inputs.file copyTask
    outputs.dir 'some-dir' //对输出做最新检查
    doLast{
        copy {
            // 复制 copyTask 的输出
            from copyTask
            into 'some-dir'
        }
    }
}

```

建议尽可能的使用复制任务,因为它支持增量化的构建和任务依赖推理，而不需要去额外的费力处理这些.不过 `copy()` 方法可以用作复制任务实现的一部分.即该 方法被在自定义复制任务中使用,请参考 [第60章 编写自定义任务](https://docs.gradle.org/current/userguide/custom_tasks.html).在这样的场景下，自定义任务应该充分声明与复制操作相关的输入/输出。

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


源文件中扩展和过滤操作都会查找的 `token`,格式类似 `@tokenName@` 被命名为 `tokenName` .

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









































