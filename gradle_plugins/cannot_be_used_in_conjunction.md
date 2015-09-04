# 不能与subjects{},allprojects{}等结合使用

目前不能使用一次添加一个插件到多个项目中的模式,如使用`subprojects{}`等方式.目前没有机制可以应用一次插件到多个项目.目前,每个项目都必须在自己的构建脚本中的`plugins{}`块中声明应用的插件.

*Gradle的未来版本将删除此限制.*

如果新语法的限制让人望而却步,推荐使用[buildscript{} block](https://docs.gradle.org/current/userguide/plugins.html#sec:applying_plugins_buildscript).


