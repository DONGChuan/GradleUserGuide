## 22.10.资源
Java插件使用[Copy](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.Copy.html)任务处理资源.它为项目每个source set增加了一个实例.可以参考[Section 15.6, "Copying files"](https://docs.gradle.org/current/userguide/working_with_files.html#sec:copying_files)获取关于copy任务的信息.

**表22.12.java插件-ProcessResources配置**

任务属性           | 类型                                                                                                                                                                  | 默认值
-------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -----------------------------
srcDirs        | Object.可以在[Section 15.5, “Specifying a set of input files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:specifying_multiple_files)中查看使用什么 | sourceSet.resources
destinationDir | File.可以再[Section 15.1, “Locating files”](https://docs.gradle.org/current/userguide/working_with_files.html#sec:locating_files)查看使用什么                                | sourceSet.output.resourcesDir
