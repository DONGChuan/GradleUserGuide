# 扩展属性
在 Gradle 领域模型中所有被增强的对象能够拥有自己定义的属性. 这包括，但不仅限于 projects , tasks , 还有 source sets . Project 对象可以添加，读取，更改扩展的属性. 另外，使用 ext 扩展块可以一次添加多个属性.

**例子 13.3. 使用扩展属性**

**build.gradle**

    apply plugin: "java"

    ext {
        springVersion = "3.1.0.RELEASE"
        emailNotification = "build@master.org"
    }

    sourceSets.all { ext.purpose = null }

    sourceSets {
        main {
            purpose = "production"
        }
        test {
            purpose = "test"
            }
        plugin {
            purpose = "production"
        }

        }

        task printProperties << {
            println springVersion
            println emailNotification
            sourceSets.matching { it.purpose == "production" }.each { println it.name }
        }

使用**gradle -q printProperties**输出结果

    > gradle -q printProperties
    3.1.0.RELEASE
    build@master.org
    main
    plugin

在上面的例子中，一个 ext 扩展块向 Project 对象添加了两个扩展属性. 名为 perpose 的属性被添加到每个 source set，然后设置 ext.purpose 等于 null ( null值是被允许的 ). 当这些扩展属性被添加后，它们就像预定义的属性一样可以被读取，更改值.

例子中我们通过一个特殊的语句添加扩展属性，当您试图设置一个预定义属性或者扩展属性，但是属性名拼写错误或者并不存在时，操作就会失败. Project 对象可以在任何地方使用其扩展属性 ，它们比局部变量有更大的作用域. 一个项目的扩展属性对其子项目也可见.

关于扩展属性更多的细节还有它的API，请看 [ExtraPropertiesExtension](http://gradle.org/docs/current/dsl/org.gradle.api.plugins.ExtraPropertiesExtension.html) 类的 API 文档说明.






