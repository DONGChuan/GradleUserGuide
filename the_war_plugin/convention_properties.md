# 公共配置

**表25.4.War插件-目录配置**

属性名称 | 类型 | 默认值 | 描述
 ----- | ---- | ---- | ----
 webAppDirName | String | src/main/webapp | 在项目目录的web应用的资源文件夹名
 webAppDir | File (read-only) | projectDir/webAppDirName | Web应用的资源路径

 这些属性由一个[WarPluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.WarPluginConvention.html)公共对象提供
