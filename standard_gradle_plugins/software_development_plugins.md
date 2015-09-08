# 软件开发插件

这些插件在您的软件开发过程中提供帮助.

**Table 22.5. Software development plugins**

| Plugin Id | 自动应用 | 协同工作 | 描述 |
| -- | -- | -- | -- |
| [announce](https://docs.gradle.org/current/userguide/announce_plugin.html) | - | - | 消息发布到自己喜欢的平台，如Twitter或Growl. |
| [build-announcements](https://docs.gradle.org/current/userguide/build_announcements_plugin.html) | announce | - | 发送本地通知关于有趣的事件在构建生命周期到你的桌面. |
| [checkstyle](https://docs.gradle.org/current/userguide/checkstyle_plugin.html) | java-base | - | 使用[Checksytle](http://checkstyle.sourceforge.net/index.html)对项目的Java源码执行质量检测,并生成报告. |
| [codenarc](https://docs.gradle.org/current/userguide/codenarc_plugin.html) | groovy-base | - | 使用[CodeNarc](http://codenarc.sourceforge.net/index.html)对项目的Groovy的源文件进行质量检测,并生成检测报告 |
| [eclipse](https://docs.gradle.org/current/userguide/eclipse_plugin.html) | - | java,groovy, scala | 生成Eclipse IDE的文件,从而能够以导入项目到Eclipse.参见[Chapter 7, Java Quickstart](https://docs.gradle.org/current/userguide/tutorial_java_projects.html) |
| [eclipse-wtp](https://docs.gradle.org/current/userguide/eclipse_plugin.html) | - | ear, war | 与eclipse插件一样,生成eclipse WPT(Web Tools Platform)配置文件, 导入到Eclipse中war/ear项目应配置与WTP工作.参见参见[Chapter 7, Java Quickstart](https://docs.gradle.org/current/userguide/tutorial_java_projects.html)|
| [findbugs](https://docs.gradle.org/current/userguide/findbugs_plugin.html) | java-base | - | 使用[FindBugs](http://findbugs.sourceforge.net/)执行项目的Java源文件质量检测,并生成检测报告 |
| [idea](https://docs.gradle.org/current/userguide/idea_plugin.html) | - | java | 生成[Intellij IDEA IDE](http://www.jetbrains.com/idea/index.html)配置文件,从而可以将项目导入IDEA。 |
| [jdepend](https://docs.gradle.org/current/userguide/jdepend_plugin.html) | java-base | - | 使用[JDepend](http://clarkware.com/software/JDepend.html)执行项目的源文件质量检测,并生成检测报告 |
| [pmd](https://docs.gradle.org/current/userguide/pmd_plugin.html) | java-base | - | 使用[PMD](http://pmd.sourceforge.net/)执行项目的源文件质量检测,并生成检测报告 |
| [project-report](https://docs.gradle.org/current/userguide/project_reports_plugin.html) | reporting-base | - | 生成一个包含关于您的Gradle构建有用信息的报告。 |
| [signing](https://docs.gradle.org/current/userguide/signing_plugin.html) | base | - | 添加数字签名档案和artifacts的能力。 |
| [sonar](https://docs.gradle.org/current/userguide/sonar_plugin.html) | - | java-base, java, jacoco | 与[Sonar](http://www.sonarsource.org/)代码质量平台整合.由[sonar-runner](https://docs.gradle.org/current/userguide/sonar_runner_plugin.html)插件提供 |

