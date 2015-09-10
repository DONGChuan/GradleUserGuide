# 外部的依赖

你可以声明许多种依赖. 其中一种是*external dependency(外部依赖)*. 这是一种在当前构建之外的一种依赖, 它被存放在远程或本地的仓库里, 比如 Maven 的库, 或者 Ivy 库, 甚至是一个本地的目录.

下面的例子讲展示如何加入外部依赖

*例子 8.2. 定义一个外部依赖*

*build.gradle*

    dependencies {
        compile group: 'org.hibernate', name: 'hibernate-core', version: '3.6.7.Final'
    }

引用一个外部依赖需要使用 group, name 和 version 属性. 根据你想要使用的库, group 和 version 可能会有所差别.

有一种简写形式, 只使用一串字符串 `"group:name:version"`.

*例子 8.3. 外部依赖的简写形式*

*build.gradle*

    dependencies {
        compile 'org.hibernate:hibernate-core:3.6.7.Final'
    }


要了解跟多关于定义并使用依赖工作的信息,参见[Section 52.4,"How to declare you dependencies"](https://docs.gradle.org/current/userguide/dependency_management.html#sec:how_to_declare_your_dependencies).

