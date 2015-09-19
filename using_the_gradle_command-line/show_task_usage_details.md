# 获取任务具体信息

执行 **gradle help --task someTask** 可以显示指定任务的详细信息. 或者多项目构建中相同任务名称的所有任务的信息.
如下例.

*例 11.12. 获取任务帮助*

*gradle -q help --task libs的输出结果*

    > gradle -q help --task libs
    Detailed task information for libs

    Paths
         :api:libs
         :webapp:libs

    Type
         Task (org.gradle.api.Task)

    Description
         Builds the JAR

这些结果包含了任务的路径、类型以及描述信息等.
