## 22.6.公共配置
Java插件会为project添加一些列的公共配置对象,如下所示,可以在构建脚本中使用这写属性,就像它们是该项目对象的属性(see [???](https://docs.gradle.org/current/userguide/java_plugin.html)).

**表22.7.Java插件-目录属性**

属性名称                   | 类型               | 默认值                             | 描述
---------------------- | ---------------- | ------------------------------- | ----------------------------
reportsDirName         | String           | reports                         | 在构建目录的生成报告的文件夹名
reportsDir             | File (read-only) | buildDir/reportsDirName         | 该目录下会生成报告
testResultsDirName     | String           | test-results                    | 在构建目录的测试结果的result.xml的存放目录名
testResultsDir         | File (read-only) | buildDir/testResultsDirName     | 测试结果的 result.xml 文件会存放在该文件夹中
testReportDirName      | String           | tests                           | 在构建目录的测试报告的文件夹名
testReportDir          | File (read-only) | reportsDir/testReportDirName    | 测试的测试报告会存放在该目录下
libsDirName            | String           | libs                            | 在构建目录下的类库文件夹名
libsDir                | File (read-only) | buildDir/libsDirName            | 该目录下存放类库
distsDirName           | String           | distributions                   | 在构建目录下的distributions文件夹名
distsDir               | File (read-only) | buildDir/distsDirName           | 该目录下存放生成的distributions
docsDirName            | String           | 在构建目录下的doc文件夹名                  |
docsDir                | File (read-only) | buildDir/docsDirName            | 该目录下存放生成的文档
dependencyCacheDirName | String           | dependency-cache                | 在构建目录下的依赖缓存文件夹名
dependencyCacheDir     | File (read-only) | buildDir/dependencyCacheDirName | 该目录用来缓存源依赖信息。

 **表22.8.Java插件-其他配置**

属性名称                | 类型                                                                                                                         | 默认值                 | 描述
------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------- | ---------------------
sourceSets          | [SourceSetContainer](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/SourceSetContainer.html)                 | Not null            | 包含项目的sourceSet
sourceCompatibility | [JavaVersion](https://docs.gradle.org/current/javadoc/org/gradle/api/JavaVersion.html).也可以使用String类型或Number类型,如'1.5' 或 1.5 | 当前使用的JVM版本          | 编译Java源码时所使用的Java兼容版本
targetCompatibility | [JavaVersion](https://docs.gradle.org/current/javadoc/org/gradle/api/JavaVersion.html).也可以使用String类型或Number类型,如'1.5' 或 1.5 | sourceCompatibility | 生成class文件的Java版本
archivesBaseName    | String                                                                                                                     | projectName         | 用于.jar文件或者.zip存档的基本名称
manifest            | [Mainfest](https://docs.gradle.org/current/javadoc/org/gradle/api/java/archives/Manifest.html)                             | an empty manifest   | 该清单中包括所有的JAR文件

  按照[JavaPluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.JavaPluginConvention.html)和[BasePluginConvention](https://docs.gradle.org/current/dsl/org.gradle.api.plugins.BasePluginConvention.html)类型提供这些属性.
