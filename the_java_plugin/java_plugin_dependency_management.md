# 依赖管理

Java 插件给项目增加了许多关于依赖的配置, 如下所示, 这些配置被分配给许多任务, 比如 compileJava 和 test 等配置

**表22.5.Java插件-依赖配置**

名称 | 扩展 | 被使用时运行的任务 | 含义
--------- | ---------- | ---- | -----------
compile | - | compileJava | 编译时的依赖
runtime | compile | - | 运行时的依赖
testCompile | compile | compileTestJava | 编译测试所需的额外依赖
testRuntime | runtime | test | 仅供运行测试的额外依赖
archives | - | uploadArchives | 项目产生的信息单元（如:jar包）
default | runtime | - | 使用其他项目的默认依赖项，包括该项目产生的信息单元以及依赖

**图22.2.Java插件-依赖配置**
![java plugin-dependency configurations](https://docs.gradle.org/current/userguide/img/javaPluginConfigurations.png)

对于每个添加到该项目的资源设置，java 插件会添加以下的依赖配置

**表22.6.Java插件-资源设置依赖关系配置**

名称 | 扩展 | 被使用时运行的任务 | 含义
--------- | ---------- | ---- | -----------
sourceSetCompile | - | compileSourceSetJava | 编译时给定资源设置的依赖
sourceSetRuntime | - | - |运行时给定资源设置的依赖

