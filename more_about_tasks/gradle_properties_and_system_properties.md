# Gradle 属性 和 system 属性
Gradle 提供了多种的方法让您可以在构建脚本中添加属性. 使用 -D 命令选项，您可以向运行 Gradle 的 JVM 传递一个 system 属性  . **Gradle** 命令的 -D 选项 和 **Java** 命令的 -D 选项有些相同的效果.

您也可以使用属性文件向您的 Project 对象中添加属性. 您可以在 Gradle 用户目录( 如果您没有在 *USER_HOME*/.gradle 配置默认设置,则由"GRADLE_USER_HOME" 环境变量定义) 或者项目目录放置一个 gradle.properties 文件.如果是多项目的话，您可以在每个子目录里都放置一个 gradle.properties 文件. gradle.properties 文件内容里的属性能够被 Project 对象访问到. 不过有一点，用户目录中的 gradle.properties 文件优先权大于项目目录中的 gradle.properties 文件.

您也可以通过 -P 命令选项直接向Project 对象中添加属性.

另外，当 Gradle 看到特别命名的 system 属性或者环境变量时，Gradle 也可以设置项目属性. 比如当您没有管理员权限去持续整合服务，还有您需要设置属性值但是不容易时，这个特性非常有用. 出于安全的原因，在这种情况下，您没法使用 -P 命令选项，您也不能修改系统级别的文件. 确切的策略是改变您持续继承构建工作的配置，增加一个环境变量设置令它匹配一个期望的模式. 对于当前系统来说，这种方法对于普通用户来说是不可见的. [\[6]](#md-anchor)

如果环境变量的名字是 ORG_GRADLE_PROJECT=somevalue, Gradle 会使用值为 somevalue 在您的 Project 对象中设定一个支持属性. 另外 Gradle 也支持 system 属性，但是使用不同的名字模式，例如 org.gradle.project.prop .

您也可以在 gradle.properties 文件中设置 system 属性.如果一个属性名的前缀为 “systemProp”，那么这个属性和它的属性值会被设置为 system 属性. 如果没有这个前缀，在多项目构建中，除了根项目会被忽略外，“systemProp.” 属性会在任何项目中设置.也就是说仅仅根项目的 gradle.properties 文件会被检查其属性的前缀是否是 “systemProp”.

**例子 14.2.通过 gradle.properties 文件设置属性

**gradle.properties**

    gradlePropertiesProp=gradlePropertiesValue
    sysProp=shouldBeOverWrittenBySysProp
    envProjectProp=shouldBeOverWrittenByEnvProp
    systemProp.system=systemValue

**build.gradle**

    task printProps << {
        println commandLineProjectProp
        println gradlePropertiesProp
        println systemProjectProp
        println envProjectProp
        println System.properties['system']

    }

<a name="md-anchor" id="md-anchor">[6]</a>. *Jenkins*, *Teamcity*, or *Bamboo* 都是 提供这个功能的 CI 服务.

**使用 gradle -q -PcommandLineProjectProp=commandLineProjectPropValue -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps 输出**

    > gradle -q -PcommandLineProjectProp=commandLineProjectPropValue    -Dorg.gradle.project.systemProjectProp=systemPropertyValue printProps
    commandLineProjectPropValue
    gradlePropertiesValue
    systemPropertyValue
    envPropertyValue
    systemValue


