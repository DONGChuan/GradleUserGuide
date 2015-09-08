# 集成插件

这些插件提供的各种运行时的技术的集成.

**Table 22.3. Integration plugins**

| Plugin Id | 自动应用 | 协同工作 | 描述 |
| -- | -- | -- | -- |
| [application](https://docs.gradle.org/current/userguide/application_plugin.html) | java, distribution | - | 增加了对运行绑定Java项目作为命令行应用的任务. |
| [ear](https://docs.gradle.org/current/userguide/ear_plugin.html) | - | java | 增加了对构建J2EE应用程序的支持. |
| [jetty](https://docs.gradle.org/current/userguide/jetty_plugin.html) | war | - | 在构建中嵌入Jetty web容器可以部署web应用.参见[Chapter 10, Web Application Quickstart](https://docs.gradle.org/current/userguide/web_project_tutorial.html) |
| maven | - | java, war | 增加了对发布artifacts到Maven仓库的支持. |
| [sogi](https://docs.gradle.org/current/userguide/osgi_plugin.html) | java-base | java | 增加了对构建OSGi支持 |
| [war](https://docs.gradle.org/current/userguide/war_plugin.html) | java | - | 增加了对组装Web应用程序WAR文件的支持.参见[Chapter 10, Web Application Quickstart](https://docs.gradle.org/current/userguide/web_project_tutorial.html) |
