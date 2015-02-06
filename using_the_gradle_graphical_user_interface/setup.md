# Setup
在设置界面，你可以配置一些常用的设置.

图 12.2 设置界面

![](http://gradle.org/docs/current/userguide/img/guiSetup.png)


* **“Current Directory”**      图形界面会默认设置您的Gradle项目的根目录(build.gradle 文件所在的目录)为当前目录.

* **“Stack Trace Output“**  这个选项可以指定当错误发生时，有多少信息可以写入到轨迹栈中，注意：在您设定轨迹栈级别后，如果"Command Line"(命令行)选项卡中，或者在"Favorites"（收藏夹）选项卡中的命令发生错误,这个设置就不会起作用了.

* **”Only Show Output When Errors Occur”**  设定当编译出问题时输出窗口才显示相关信息.

* **"Use Custom Gradle Executor"**  高级功能，您可以指定一个路径启动Gradle命令代替默认的设置，例如您的项目需要在别的批处理文件或者shell脚本进行额外的配置（例如指定一个初始化脚本），这种情况您就可以使用它.

