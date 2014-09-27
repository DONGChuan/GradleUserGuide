#  项目列表

执行 **gradle projects** 命令会为你列出子项目名称列表.

例 10.7. 收集项目信息

**gradle -q projects** 命令的输出结果

    > gradle -q projects
    ------------------------------------------------------------
    Root project
    ------------------------------------------------------------

    Root project 'projectReports'
    +--- Project ':api' - The shared API for the application
    \--- Project ':webapp' - The Web application implementation

    To see a list of the tasks of a project, run gradle <project-path>:tasks
    For example, try running gradle :api:tasks

这份报告展示了每个项目的描述信息.
当然你可以在项目中用 **description** 属性来指定这些描述信息.

例 10.8. 为项目添加描述信息.

*build.gradle*

    description = 'The shared API for the application'
