# 使用其他的脚本配置项目
您还可以使用其他的构建脚本来配置当前的项目，Gradle 构建语言的所有的内容对于其他的脚本都是可以使用的. 您甚至可以在别的脚本中再使用其他的脚本.

**例子 14.3.使用其他的构建脚本配置项目**

**build.gradle**

    apply from: 'other.gradle'

**other.gradle**

    println "configuring $project"
    task hello << {
        println' 'hello form other srcipt'
    }

**使用 gradle -q hello 输出**

    > gradle -q hello
    configuring root project 'configureProjectUsingScript'
    hello from other script
