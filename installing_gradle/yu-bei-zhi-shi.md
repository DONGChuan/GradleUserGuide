# 预备知识

Gradle 可以在所有的主流操作系统上运行. 只需要安装 [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 以及更高的版本. 用户可以通过运行 `java -version` 命令来查看Java JDK 的版本:

```
❯ java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

Gradle 自带 Groovy library, 所以用户不需要安装 Groovy. 所有已经安装的 Groovy 都会被 Gradle 忽略.

Gradle uses whatever JDK it finds in your path. Alternatively, you can set the`JAVA_HOME`environment variable to point to the installation directory of the desired JDK.



