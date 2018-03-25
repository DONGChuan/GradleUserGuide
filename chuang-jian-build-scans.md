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

这样就可以非常简单的创建 ad-hoc, 一次性的 build scans 而不需要进行额外的配置. If you need finer grained configuration, you can configure the build scan plugin in a build or init script as described in the following sections.

## [Enable build scans on all builds of your project](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#enable_build_scans_on_all_builds_of_your_project) {#enable_build_scans_on_all_builds_of_your_project}

Add a`plugins`block to the`build.gradle`file with the following contents:

```
plugins {
    id 
'
com.gradle.build-scan
'
 version 
'
1.12.1
'

}
```

|  | Use latest plugin version which can be found on the[Gradle Plugin Portal](https://plugins.gradle.org/plugin/com.gradle.build-scan). |
| :--- | :--- |


If you already have a`plugins`block, always put the build scan plugin first. Adding it below any existing plugins will still work, but will miss useful information.

## [Accept the license agreement](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#accept_the_license_agreement) {#accept_the_license_agreement}

In order to publish build scans to[https://scans.gradle.com](https://scans.gradle.com/), you need to accept the license agreement. This can be done ad-hoc via the command line when publishing, but can also be specified in your Gradle build file, by adding the following section:

```
buildScan {
    licenseAgreementUrl = 
'
https://gradle.com/terms-of-service
'

    licenseAgree = 
'
yes
'

}
```

The`buildScan`block allows you to configure the plugin. Here you are setting two properties necessary to accept the license agreement. Other properties are available. See the[Build Scans User Manual](https://docs.gradle.com/build-scan-plugin/)for details.

## [Publish a build scan](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#publish_a_build_scan) {#publish_a_build_scan}

A build scan is published using a command-line flag called`--scan`.

Run a`build`task with the`--scan`option. When the build is completed, after uploading the build data to scans.gradle.com, you will be presented with a link to see your build scan.

```
$ ./gradlew build --scan

BUILD SUCCESSFUL in 5s

Publishing build scan...
https://gradle.com/s/47i5oe7dhgz2c
```

## [Access the build scan online](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#access_the_build_scan_online) {#access_the_build_scan_online}

The first time you follow the link, you will be asked to activate the created build scan.

The email you receive to activate your build scan will look similar to:

![](https://guides.gradle.org/creating-build-scans/images/build_scan_email.png "build scan email")

Follow the link provided in the email, and you will see the created build scan.

![](https://guides.gradle.org/creating-build-scans/images/build_scan_page.png "build scan page")

You can now explore all the information contained in the build scan, including the time taken for tasks to execute, the time required during each stage of the build, the results of any tests, plugins used and other dependencies, any command-line switches used, and more.

## [Enable build scans for all builds \(optional\)](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#enable_build_scans_for_all_builds_optional) {#enable_build_scans_for_all_builds_optional}

You can avoid having to add the plugin and license agreement to every build by using a Gradle init script. Create a file called`buildScan.gradle`in the directory`~/.gradle/init.d`\(where the tilde represents your home directory\) with the following contents:

```
initscript {
    repositories {
        maven { url 
'
https://plugins.gradle.org/m2
'
 }
    }

    dependencies {
        classpath 
'
com.gradle:build-scan-plugin:1.12.1
'

    }
}

rootProject {
    apply 
plugin
: com.gradle.scan.plugin.BuildScanPlugin

    buildScan {
        licenseAgreementUrl = 
'
https://gradle.com/terms-of-service
'

        licenseAgree = 
'
yes
'

    }
}
```

The init script downloads the build scan plugin if necessary and applies it to every project, and accepts the license agreement. Now you can use the`--scan`flag on any build on your system.

There are additional capabilities you can add to the script, such as under what conditions to publish the scan information. For details, see the[Build Scans User Manual](https://docs.gradle.com/build-scan-plugin/).

## [Summary](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#summary) {#summary}

In this guide, you learned how to:

* Generate a build scan

* View the build scan information online

* Create an init script to enable build scans for all builds

## [Next steps](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#next_steps) {#next_steps}

Additional information can be found in the[Build Scans User Manual](https://docs.gradle.com/build-scan-plugin/).

## [ ](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#help_improve_this_guide) {#help_improve_this_guide}



