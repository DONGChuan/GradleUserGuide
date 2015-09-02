# 为什么会在机器上出现不只一个守护进程

有几个原因Gradle会创建一个新的守护进程代替使用一个已存在的守护进程.如果守护进程没有*闲置*,*兼容*,则会启动一个新的守护进程.

*空闲*的守护进程是当前未执行构建或做其他有用的工作.

*兼容*的守护进程是一个可以（或者可以达到）满足要求的编译环境的要求。Java安装程序运行的构建是构建环境方面的一个例子。构建运行时所需的JVM系统属性是另一个例子。

一个已经运行的Java进程可能不能满足所需的构建环境的某些方面。如果守护进程由Java7启动，但要求的环境要求为Java8,则守护进程是不兼容的，必须另外启动。再者，在运行的JVM不能改变一个运行时的某些性能。如内存分配（如-Xmx1024m），默认文本编码运行的JVM中，默认的语言环境，等等一个JVM不能改变的运行环境。

"Required build environment"通常在构建客户端(如Gradle命令行,IDE等)方面隐含构建环境,并明确通过命令行选项设置.参见[Chapter 20,The Build Environment](https://docs.gradle.org/current/userguide/build_environment.html)有关如何指定和控制构建环境的详细信息.

一下JVM系统属性是有效不变的.如果需求编译环境需要这些属性,不同的守护进程JVM在下列属性中有不同的值时,守护进程不兼容.

+ file.encoding
+ user.language
+ user.country
+ user.variant
+ com.sun.management.jmxremote

下列JVM属性,通过启动参数控制,也是有效不变的.在需求构建环境和守护进程环境的对应属性必须按顺序完全匹配,才可兼容.

+ 最大堆大小(即 -Xmx JVM参数)
+ 最小堆大小(即 -Xms JVM参数)
+ 引导类路径(即 -Xbootclasspath JVM参数)
+ "assertion"状态(即 -ea 参数)

所需的Gradle版本是需求构建环境的另一个方面.守护进程被耦合到特定Gradle运行时,多个正在运行的守护进程产生的原因是使用使用不同版本的Gradle会在会话过程中处理多个项目.



