# JVM 选项

JVM 选项可以通过设置环境变量来更改. 您可以使用 GRADLE_OPTS 或者 JAVA_OPTS. 根据惯例, JAVA_OPTS是一个用于JAVA应用的环境变量. 一个典型的用例是在 JAVA_OPTS 里设置HTTP代理服务器(proxy) , 在 GRADLE_OPTS 这是内存选项. 这些变量也可以在gradle的一开始就设置或者通过 gradlew 脚本.
