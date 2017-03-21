# Ear 插件 (未完成)

Ear插件添加了对组装Web应用程序EAR文件的支持。 它将添加默认EAR文件生成任务。 它不需要Java插件，对于使用Java插件的项目，它会禁用默认的JAR文件生成。

## 51.1.使用
使用ear插件需要在构建脚本下包括以下内容

**例51.1.使用ear插件**

**build.gradle**

```
apply plugin 'ear'
```

## 51.2.任务
War插件会添加下列任务到项目.

**表51.2.War插件-任务**

任务名     | 依赖于        |  类型 | 描述
--------- | ---------- | ---- | -----------
ear | compile (仅在java插件存在时) | [Ear](https://docs.gradle.org/current/dsl/org.gradle.plugins.ear.Ear.html) | 组装应用程序Ear文件

Ear插件由其他已存在的插件添加下列依赖任务.

**表51.2.Ear插件-附加的依赖任务**

任务名    | 依赖于
-------- | ------
assemble | ear


## 51.3.项目布局

**表25.3.Ear插件-项目布局**

文件夹   | 含义
-------- | ------
src/main/application | Ear资源,比如**META-INF**目录

## 51.4.依赖管理
Ear插件添加了两个依赖配置：**deploy**和**earlib**。 **deploy**中的所有依赖关系都放在EAR存档的根目录中，并且是不可传递的（not transitive）。 **earlib**配置中的所有依赖关系都放在EAR存档中的'lib'目录中，并且是可传递的（transitive）。

## 51.5.常用配置

**表51.4.Ear插件-目录配置**

属性名称 | 类型 | 默认值 | 描述
 ----- | ---- | ---- | ----
 appDirName | String | src/main/application | 应用的资源文件夹，为与项目的相对路径
 libDirName | String | lib | 生成的ear文件中lib目录的名字
 deploymentDescriptor | org.gradle.plugins.ear.descriptor.DeploymentDescriptor | 在一个默认的构部署配置文件里：application.xml | 生成部署描述符文件的元数据，例如 application.xml。 如果此文件已存在于appDirName / META-INF中，则将使用现有文件内容，并忽略ear.deploymentDescriptor中的显式配置。

 这些属性由一个[WarPluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.WarPluginConvention.html)公共对象提供

## 25.6.War
War任务默认会把`src/main/webapp`的内容复制到归档目录的根目录.webapp文件夹下会包含一个`WEB-INF`子文件夹,里面可能会有一个web.xml文件.编译后的class文件会在`WEB-INF/classes`下,所有runtime<sup>[[13](https://docs.gradle.org/current/userguide/war_plugin.html#ftn.N1325D)]</sup>的依赖配置会被拷贝至`WEB-INF/lib`下.

API文档中有更多关于[War](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.War.html)的信息.

## 25.7.定制War
下面的例子中有一些重要的自定义选项

**例25.2.定制War插件**

**build.gradle**

```
configuration{
  moreLibs
}

respositories{
  faltDir {dirs "lib"}
  mavenCentral()
}

dependencies{
  compile module(":compile:1.0") {
        dependency ":compile-transitive-1.0@jar"
        dependency ":providedCompile-transitive:1.0@jar"
  }
  providedCompile "javax.servlet:servlet-api:2.5"
    providedCompile module(":providedCompile:1.0") {
        dependency ":providedCompile-transitive:1.0@jar"
  }
  runtime ":runtime:1.0"
  providedRuntime ":providedRuntime:1.0@jar"
  testCompile "junit:junit:4.12"
  moreLibs ":otherLib:1.0"
}

war{
  from 'src/rootContent' // 增加一个目录到归档根目录
  webInf {from 'src/additionalWebInf'} // 增加一个目录到 WEB-INF 下
  classpath fileTree('additionalLibs') // 增加一个目录到 WEB-INF/lib下.
  classpath configurations.moreLibs // 增加更多地设置到 WEB-INF/lib 下.
  webXml = file('src/someWeb.xml') // 复制xml文件到 WEB-INF/web.xml.
}
```

当然,可以用一个封闭的标签定义一个文件是否存打包到War文件中.

---
[[13](https://docs.gradle.org/current/userguide/war_plugin.html#N1325D)]`runtime`配置将会继承`compile`配置.
