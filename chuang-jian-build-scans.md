# 创建 Build Scans

build scan 是一种可分享的, 构建的集中记录. 通过它, 你可以对项目发生了什么以及为什么有一个全局的了解. 通过在项目中加入 build scan 插件, 你可以免费将 build scans 发布到 [https://scans.gradle.com](https://scans.gradle.com/) .

这篇教程将展示:

1. 如何不改编任何构建脚本来发布 build scans ad-hoc. 
2. 如何更改构建脚本来使用对整个项目的所有构建使用 build scans 功能.

### 选择一个示例项目

Gradle 提供了一个简单的 Java 项目给用户来尝试 build scan. 下载地址 [https://github.com/gradle/gradle-build-scan-quickstart](https://github.com/gradle/gradle-build-scan-quickstart). 你可以使用自己的项目.

### 自动使用 build scan 插件

从 Gradle 4.3 开始, 你可以直接使用 build scans, 不再需要添加额外的配置. 当使用命令行选项 `--scan` 来发布一个 build scan,  build scan 插件就会自动被运行. 在构建结束之前, 你会被询问是否同意接受 license agreement:

```
$ ./gradlew build --scan

BUILD SUCCESSFUL in 6s

Do you accept the Gradle Cloud Services license agreement (https://gradle.com/terms-of-service)? [yes, no]
yes
Gradle Cloud Services license agreement accepted.

Publishing build scan...
https://gradle.com/s/czajmbyg73t62
```

这样就可以非常简单的创建点对点形式的, 一次性的 build scans 而不需要进行额外的配置. 如果你需要更优化的配置方案, 你可以跟随下面的教程.

### 在你项目的所有构建里都启动 build scans

在`build.gradle`文件里加入以下代码:

```
plugins {
    id 'com.gradle.build-scan' version '1.12.1'
}
```

如果你已经有`plugins` 代码块了, 请把 build scan 插件放在第一个. 如果加在其它插件下面, 它任然能工作, 但是许多有用的信息可能就扫描不到了.

### 接受 license agreement

为了发布扫描结果到 [https://scans.gradle.com](https://scans.gradle.com/), 你需要接受 license 协议. 可以在发布的时候通过命令行接受, 也可以在 Gradle build 文件里提前配置好:

```
buildScan {
    licenseAgreementUrl = 'https://gradle.com/terms-of-service'
    licenseAgree = 'yes'
}
```

`buildScan`代码块是专门用来配置这个插件的. 这里仅列举了2个属性, 其余属性可以参见 [Build Scans User Manual](https://docs.gradle.com/build-scan-plugin/).

### 发布 build scan

只需要添加在命令行末尾添加`--scan就可以发布` build scan.

运行`build --scan`任务. 当构建完成, 你就可以通过产生的链接去查看了

```
$ ./gradlew build --scan

BUILD SUCCESSFUL in 5s

Publishing build scan...
https://gradle.com/s/47i5oe7dhgz2c
```

### 查看网上的 build scan

你第一次打开链接的时候, 需要通过邮箱激活, 邮件类似下图:

![](https://guides.gradle.org/creating-build-scans/images/build_scan_email.png "build scan email")

点击邮件内的链接, 就可以查看 build scan 了:

![](https://guides.gradle.org/creating-build-scans/images/build_scan_page.png "build scan page")

## 对系统上的所有构建都激活 Build Scan {#enable_build_scans_for_all_builds_optional}

你并不需要在项目里的每一个构建里都进行上述的操作. 你可以在HOME下面的`~/.gradle/init.d`创建一个叫做 `buildScan.gradl`的文件:

```
initscript {
    repositories {
        maven { url 'https://plugins.gradle.org/m2'}
    }

    dependencies {
        classpath 'com.gradle:build-scan-plugin:1.12.1'
    }
}

rootProject {
    apply plugin: com.gradle.scan.plugin.BuildScanPlugin

    buildScan {
        licenseAgreementUrl = 'https://gradle.com/terms-of-service'
        licenseAgree = 'yes'
    }
}
```

然后, 在该系统的任何构建里, 你只需要添加`--scan` 就可以直接使用 Build Scan 了.

### 下一步

可以查看 [Build Scans User Manual](https://docs.gradle.com/build-scan-plugin/) 获得更多关于该插件的信息

## [ ](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#help_improve_this_guide) {#help_improve_this_guide}



