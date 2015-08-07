# 22.4.项目布局

Java 插件的默认布局如下图所示，无论这些文件夹中有没有内容，Java插件都会编译里面的内容，并处理缺失的内容。

**表22.4.java 插件-默认布局**

目录                      | 含义
----------------------- | --------------------
src/main/java           | 主要 Java 源码
src/main/resources      | 主要资源
src/test/java           | 测试 Java 源码
src/test/resources      | 测试资源
src/sourceSet/java      | 指定sourceSet的 Java 源码
src/sourceSet/resources | 指定sourceSet的资源
