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

 这些属性由一个[EarPluginConvention](https://docs.gradle.org/current/dsl/org.gradle.plugins.ear.EarPluginConvention.html)公共对象提供
**-/-/-/-/-//-/-/-/--/-/-/-/-/-当前进度↑↑-/-/-/-/-/--/-/-/-/-/-/-/-/-/**
## 51.6.Ear
War任务默认会把`src/main/application`的内容复制到归档目录的根目录。如果配置文件**META-INF/application.xml**不存在，它将被自动生成。

API文档中有更多关于[Ear](https://docs.gradle.org/current/dsl/org.gradle.plugins.ear.Ear.html)的信息.

## 51.7.定制Ear
下面的例子中有一些重要的自定义选项

**例25.2.定制Ear插件**

**build.gradle**

```
pply plugin: 'ear'
apply plugin: 'java'

repositories { mavenCentral() }

dependencies {
    // The following dependencies will be the ear modules and
    // will be placed in the ear root
    deploy project(path: ':war', configuration: 'archives')

    // The following dependencies will become ear libs and will
    // be placed in a dir configured via the libDirName property
    earlib group: 'log4j', name: 'log4j', version: '1.2.15', ext: 'jar'
}

ear {
    appDirName 'src/main/app'  // use application metadata found in this folder
    // put dependent libraries into APP-INF/lib inside the generated EAR
    libDirName 'APP-INF/lib'
    deploymentDescriptor {  // custom entries for application.xml:
//      fileName = "application.xml"  // same as the default value
//      version = "6"  // same as the default value
        applicationName = "customear"
        initializeInOrder = true
        displayName = "Custom Ear"  // defaults to project.name
        // defaults to project.description if not set
        description = "My customized EAR for the Gradle documentation"
//      libraryDirectory = "APP-INF/lib"  // not needed, above libDirName setting does this
//      module("my.jar", "java")  // won't deploy as my.jar isn't deploy dependency
//      webModule("my.war", "/")  // won't deploy as my.war isn't deploy dependency
        securityRole "admin"
        securityRole "superadmin"
        withXml { provider -> // add a custom node to the XML
            provider.asNode().appendNode("data-source", "my/data/source")
        }
    }
}
```
你也可以使用一些**Ear**任务提供的自定义选项，如**from**和**metaInf**.
## 51.8.使用自定义的配置文件


