## 22.5.依赖管理
Java插件给project增加了相关配置，如下所示，这些配置被分配成像compileJava和test等配置

**表22.5.Java插件-依赖配置**

名称          | 扩展      | 运行任务            | 含义
----------- | ------- | --------------- | -----------------------------
compile     | -       | compileJava     | 编译时的依赖
runtime     | compile | -               | 运行时的依赖
testCompile | compile | compileTestJava | 编译测试所需的额外依赖
testRuntime | runtime | test            | 仅供运行测试的额外依赖
archives    | -       | uploadArchives  | 项目产生的信息单元（如:jar包）
default     | runtime | -               | 使用其他项目的默认依赖项，包括该项目产生的信息单元以及依赖

**图22.2.Java插件-依赖配置** ![java plugin-dependency configurations](https://docs.gradle.org/current/userguide/img/javaPluginConfigurations.png)

对于每个添加到该项目的sourceSet，java插件会添加一下的依赖配置

**表22.6.Java插件-sourceSet依赖关系配置**

名称               | 扩展 | 被使用时运行的任务            | 含义
---------------- | -- | -------------------- | -----------------
sourceSetCompile | -  | compileSourceSetJava | 编译时给定sourceSet的依赖
sourceSetRuntime | -  | -                    | 运行时给定sourceSet的依赖
