# 闭包作为方法的最后一个参数
Gradle DSL 在很多地方使用闭包，这里我们将讨论更多关于闭包的使用. 当一个方法的最后一个参数是一个闭包时，您可以在方法调用后放置一个闭包.

**例子: 13.8.闭包作为方法的参数**

**build.gradle**

    repositories {
        println "in a closure"
    }
    repositories() { println "in a closure" }
    repositories({println "in a closure" })
