# 基础插件

这些插件是形成其他插件的基本构建模块.你可以在你的构建文件中使用它们,在下面李处完整地列表,然而,注意它们还不是Gradle的公用API的一部分.因此,这些插件未记录在用户指南中.你可能会参考他们的API文档,详细了解它们.

**Table 22.7. Base plugins**

#### base

添加标准的生命周期任务和配置合理的默认归档任务:
+ 增加*ConfigurationName*任务.这些任务组装指定配置的工件。
+ 增加了上传*ConfigurationName*任务,这些任务组装并上传指定配置的工件。
+ 对所有归档任务配置合理的默认值(如继承[AbstractArchiveTask](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.AbstractArchiveTask.html)的任务).如归档类型的任务:[Jar](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html),[Tar](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Tar.html),[Zip](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Zip.html).特别的,归档的`destinationDir`,`baseName`和`version`属性是预先配置的默认值.这是非常有用的,因为它推动了跨项目的一致性;关于档案的命名规则和完成构建后的位置的一致性。

#### java-base

+ 增加资源集的概念到项目中.不添加特定的资源.

#### groovy-base

+ 增加了Groovy的源集理念到项目中.

#### scala-base

+ 添加scala源集合概念到项目中.

#### reporting-base

+ 为项目增加了一些涉及到生产报告的公约性质的属性,


> 译者注:实在不会使用MarkDown在表格中加入列表,好在只表格只有两列,故本节不能按照官网的User Guide表格给出.而是以列表形式完成
