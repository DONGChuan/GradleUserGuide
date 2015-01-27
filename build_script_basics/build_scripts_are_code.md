# 构建脚本代码

Gradle 的构建脚本展示了 Groovy 的所有能力. 作为开胃菜, 来看看这个:

*Example 5.4. 在 Gradle 任务里使用 Groovy *

*build.gradle*

    task upper << {
        String someString = 'mY_nAmE'
        println "Original: " + someString
        println "Upper case: " + someString.toUpperCase()
    }

**gradle -q upper** 命令的输出

    > gradle -q upper
    Original: mY_nAmE
    Upper case: MY_NAME

或者

*Example 5.5. 在 Gradle 任务里使用 Groovy*

*build.gradle*

    task count << {
        4.times { print "$it " }
    }

**gradle -q count** 命令的输出

    > gradle -q count
    0 1 2 3
