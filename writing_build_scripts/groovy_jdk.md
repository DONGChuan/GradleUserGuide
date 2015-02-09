# Groovy JDK
Groovy 在 Java 基础上添加了很多有用的方法. 例如，Iterable 有一个 each 方法， 通过使用 each 方法,我们可以迭代出 Iterable 中的每一个元素:

**例子: 13.4.Groovy JDK 方法**

**build.gradle**

    configuration.runtime.each { File f -> println f }

更多内容请阅读 http://groovy.codehaus.org/groovy-jdk/

