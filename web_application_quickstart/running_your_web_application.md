# Running your web application

要启动Web工程,在项目中加入Jetty plugin即可:

*例 9.2. 采用Jetty plugin启动web工程*

*build.gradle*

    apply plugin: 'jetty'

由于Jetty plugin继承自War plugin.使用 **gradle jettyRun** 命令将会把你的工程启动部署到jetty容器中. 调用 **gradle jettyRunWar** 命令会打包并启动部署到jetty容器中.

TODO: which url, configure port, uses source files in place and can edit your files and reload.
