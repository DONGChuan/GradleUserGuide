# War 插件 (未完成)

WAR插件扩展了Java插件,支持web应用组装成War文件.它默认禁用了Java插件JAR归档任务，并增加了一个默认的WAR归档任务。

## 25.1.使用
使用war插件需要在构建脚本下包括以下内容

**例25.1.使用war插件**

**build.gradle**

```
apply plugin 'war'
```

## 25.2.任务
War插件会添加下列任务到项目.

**表25.1.War插件-任务**

任务名     | 依赖        |  类型 | 描述
--------- | ---------- | ---- | -----------
war | compile | [War](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.War.html) | 组装应用程序War文件

War插件由Java插件添加下列依赖任务.

**表25.2.War插件-附加的依赖任务**

任务名    | 依赖
-------- | ------
assemble | war

**图25.1.War插件-任务**
![war plugin-tasks](https://docs.gradle.org/current/userguide/img/warPluginTasks.png)

## 25.3.项目布局

**表25.3.War插件-项目布局**
文件夹    | 含义
-------- | ------
src/main/webapp | Web应用资源

## 25.4.依赖管理
War插件增加了名为providedCompile和providedRuntime的两个依赖配置.这两个配置有相同的作用域在编译或者运行时的配置,不同之处在于是否会将war文件归档.很重要的一点是它们都会提供配置传递.比如在任意的provided配置中添加了`commons-httpclient:commons-httpclient:3.0`,该依赖依赖于`commons-codec`,因为这个一个"provided"的配置,意味着这两个依赖都不会被加入你的WAR中,即使`commons-codec`库是一个显式的编译配置.如果不希望出现这种传递行为,`commons-httpclient:commons-httpclient:3.0@jar`这样声明provided依赖即可.

## 25.5.公共配置

**表25.4.War插件-目录配置**

属性名称 | 类型 | 默认值 | 描述
 ----- | ---- | ---- | ----
 webAppDirName | String | src/main/webapp | 在项目目录的web应用的资源文件夹名
 webAppDir | File (read-only) | projectDir/webAppDirName | Web应用的资源路径

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
[[13](https://docs.gradle.org/current/userguide/war_plugin.html#N1325D)]`runtime`配置扩展了`compile`配置.
