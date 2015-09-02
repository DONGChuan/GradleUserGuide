# 二进制插件的位置

一个插件是一个简单的实现了[插件](https://docs.gradle.org/current/javadoc/org/gradle/api/Plugin.html)接口的类.Gradle提供的核心插件作为其分布的一部分,因此,你需要做的仅仅是应用上述的插件.然而,非核心二进制插件需要到构建类路径才能应用它们.这可以以多种方式，包括以下方式实现:

* 定义插件作为构建脚本中内嵌类的声明.
* 定义插件为在项目中buildSrc目录下的一个源文件.(参见[Section 62.4, “Build sources in the buildSrc project”](https://docs.gradle.org/current/userguide/organizing_build_logic.html#sec:build_sources))
* 包含来自外部jar定义的构建脚本依赖插件(参见[Section 21.4, “Applying plugins with the buildscript block”](https://docs.gradle.org/current/userguide/plugins.html#sec:applying_plugins_buildscript))
* 包含插件DSL语言组成的插件门户网站([Section 21.5, “Applying plugins with the plugins DSL”](https://docs.gradle.org/current/userguide/plugins.html#sec:plugins_block))

欲了解有关自定义插件的跟多信息,参见[Chapter 61, Writing Custom Plugins](https://docs.gradle.org/current/userguide/custom_plugins.html)
