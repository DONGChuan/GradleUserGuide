# 标准项目属性
Project 对象提供了一些标准的属性，您可以在构建脚本中很方便的使用他们. 下面列出了常用的属性:

Name | Type  | Default Value
:------|:------|:------
project  | [Project](http://gradle.org/docs/current/dsl/org.gradle.api.Project.html)|Project 实例对象
name  | String | 项目目录的名称
path  | String | 项目的绝对路径
description| String | 项目描述
projectDir | File | 包含构建脚本的目录
build | File | *projectDir*/build
group | Object | 未具体说明
version | Object | 未具体说明
ant |[AntBuilder](http://gradle.org/docs/current/javadoc/org/gradle/api/AntBuilder.html) | Ant实例对象

**建议：**

不要忘记我们的构建脚本只是个很简单的 Groovy 代码 ，不过它会再调用 **Gradle API**，[Project](http://gradle.org/docs/current/dsl/org.gradle.api.Project.html) 接口通过调用 ** Gradle API ** 让我们可以操作任何事情，因此如果你想知道哪个标签('tags') 可以在构建脚本种使用，您可以翻阅 Project 接口的的说明文档.


