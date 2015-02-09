# 闭合委托对象
每个闭合都有一个委托对象，当闭合既不是局部变量也不是作为方法参数时，Groovy 使用委托对象查找变量和方法引用. 当委托对象被用来管理时，Gradle 使用它来管理闭合.

**例子 13.9.闭合引用**

**build.gradle**

    dependencies {
        assert delegate == project.dependencies
        testCompile('junit:junit:4.11')
        delegate.testCompile('junit:junit:4.11')
    }
