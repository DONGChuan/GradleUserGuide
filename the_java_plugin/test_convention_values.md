### 22.13.8.公共值
**表22.14.Java插件-测试属性**

任务属性           | 类型                                                                                                                                                                                                                                                    | 默认值
-------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------
testClassesDir | File | sourceSets.test.output.classesDir
classpath | [FileCollection](https://docs.gradle.org/2.4/javadoc/org/gradle/api/file/FileCollection.html) | sourceSets.test.runtimeClasspath
testResultsDir | File | testResultsDir
testReportDir | File | testReportDir
testSrcDirs | List<File> | sourceSets.test.java.srcDirs
