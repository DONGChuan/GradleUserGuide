# 创建归档文件

一个项目可以有很多 JAR 文件,你可以向项目中添加 WAR , ZIP 和 TAR 文档,使用归档任务可以创建这些文档: Zip , Tar , Jar , War 和Ear. 它门都以同样的机制工作.

**例 15.19 创建一个 ZIP 文档 **

**build.gradle**

```
apply plugin: 'java'

task zip(type: Zip) {
    from 'src/dist'
    into('libs') {
        from configurations.runtime
    }
}

```

所有的归档任务的工作机制和复制任务相同,它们都实现了 `CopySpec` 接口,和 `Copy` 任务一样,使用 `from()` 方法指定输入文件,可以选择性的使用 `into()` 方法指定什么时候结束.你还可以过滤文件内容,重命名文件等等,就如同使用复制规则一样.

##重命名文档



`projectName-vsersion.type` 格式可以被用来生成文档名,例如:

**例 15.20 创建压缩文档**

**build.gradle**

```
apply plugin: 'java'

version = 1.0

task myZip(type: Zip) {
    from 'somedir'
}

println myZip.archiveName
println relativePath(myZip.destinationDir)
println relativePath(myZip.archivePath)

```
使用 `gradle -q myZip` 命令进行输出:
```
> gradle -q myZip
zipProject-1.0.zip
build/distributions
build/distributions/zipProject-1.0.zip

```

上面例子使用一个名为 myZip的 Zip 归档任务生成名称为 zipProject-1.0.zip 的 ZIP 文档,区分文档任务名和最终生成的文档名非常的重要

可以通过设置项目属性 `archivesBaseName` 的值来 修改生成文档的默认名.当然,文档的名称也可以通过其他方法随时更改.

归档任务中有些属性可以配置,如表 15.1 所示:

**表 15.1 归档任务-属性名**


属性名 | 类型 | 默认值 | 描述
-------|------|---------|----
archiveName     | String | baseName-appendix-version-classifier.extension,如果其中任何一个都是空的,则不添加名称|归档文件的基本文件名
archivePath     | File   | destinationDir/archiveName |生成归档文件的绝对路径。
destinationDir	| File   | 取决于文档类型, JAR 和 WAR 使用project.buildDir/distributions. project.buildDir/libraries.ZIP 和 TAR|归档文件的目录
baseName	    | String | project.name|归档文件名的基础部分。
appendix	    | String | null|归档文件名的附加部分。
version	        | String | project.version|归档文件名的版本部分。
classifier	    | String | null|归档文件名的分类部分
extension	    | String | 取决于文档类型和压缩类型: zip, jar, war, tar, tgz 或者 tbz2.|归档文件的扩展名


**例 15.21 配置归档文件-自定义文档名**

**build.gradle**

```
apply plugin: 'java'
version = 1.0

task myZip(type: Zip) {
    from 'somedir'
    baseName = 'customName'
}

println myZip.archiveName

```

使用 `gradle -q myZip` 命令进行输出:

```
> gradle -q myZip
customName-1.0.zip
```
更多配置:

**例 15.22 配置归档任务- 附加和 分类**

**build.gradle**

```
apply plugin: 'java'
archivesBaseName = 'gradle'
version = 1.0

task myZip(type: Zip) {
    appendix = 'wrapper'
    classifier = 'src'
    from 'somedir'
}

println myZip.archiveName

```

使用 `gradle -q myZip` 命令进行输出:

```
> gradle -q myZip
gradle-wrapper-1.0-src.zip

```

##在不同归档文件间分享内容

You can use the Project.copySpec() method to share content between archives.

Often you will want to publish an archive, so that it is usable from another project. This process is described in Chapter 53, Publishing artifacts.

你可以使用 `Project.copySpec()` 在不用归档文件间分享内容.
如果你想发布一个文档,然后在另一个项目中使用，这部分将在第 53 章 [发布文档](https://docs.gradle.org/current/userguide/artifact_management.html) 讲述








































