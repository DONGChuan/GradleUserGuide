# 先决条件

Gradle 需要运行在一个 Java 环境里

* 安装一个 Java JDK 或者 JRE. 而且 Java 版本必须至少是 6 以上.
* Gradle 自带 Groovy 库, 所以没必要安装 Groovy. 任何已经安装的 Groovy 会被 Gradle 忽略.

Gradle 使用任何已经存在在你的路径中的 JDK (可以通过 **java -version** 检查, 如果有就说明系统已经安装了 Java 环境). 或者, 你也可以设置 JAVA_HOME 环境参数来指定希望使用的JDK的安装目录.
