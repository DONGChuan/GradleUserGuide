# 创建 Build Scans

A build scan is a shareable and centralized record of a build that provides insights into what happened and why. By applying the build scan plugin to your project, you can publish build scans to[https://scans.gradle.com](https://scans.gradle.com/)for free.

Contents

* [What you’ll create](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#what_you_ll_create)
* [What you’ll need](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#what_you_ll_need)
* [Select a sample project](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#select_a_sample_project)
* [Auto-apply the build scan plugin](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#auto_apply_the_build_scan_plugin)
* [Enable build scans on all builds of your project](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#enable_build_scans_on_all_builds_of_your_project)
* [Accept the license agreement](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#accept_the_license_agreement)
* [Publish a build scan](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#publish_a_build_scan)
* [Access the build scan online](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#access_the_build_scan_online)
* [Enable build scans for all builds \(optional\)](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#enable_build_scans_for_all_builds_optional)
* [Summary](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#summary)
* [Next steps](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#next_steps)
* [Help improve this guide](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#help_improve_this_guide)

## [What you’ll create](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#what_you_ll_create) {#what_you_ll_create}

This guide shows you how to publish build scans ad-hoc without any build script modifications. You will also learn how to modify a build script to enable build scans for all builds of a given project. Optionally, you will also modify an init script to enable build scans for all of your projects.

## [What you’ll need](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#what_you_ll_need) {#what_you_ll_need}

* Either your own sample project, or you can use the sample project available from Gradle

* Access to the Internet

* Access to your email

* About7 minutes

## [Select a sample project](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#select_a_sample_project) {#select_a_sample_project}

Gradle makes available a simple Java project that you can use to demonstrate build scan capabilities. If you wish to use it, clone or download the repository located at[https://github.com/gradle/gradle-build-scan-quickstart](https://github.com/gradle/gradle-build-scan-quickstart). If you prefer to use your own project, you can skip this step.

## [Auto-apply the build scan plugin](https://guides.gradle.org/creating-build-scans/?_ga=2.115847618.599696663.1521685504-557532416.1521019880#auto_apply_the_build_scan_plugin) {#auto_apply_the_build_scan_plugin}

Starting with Gradle 4.3, you can enable build scans without any additional configuration in your build script. When using the command line option`--scan`to publish a build scan, the required build scan plugin is applied automatically. Before the end of the build, you are asked to accept the license agreement on the command line. The following console output demonstrates the behavior.

```
$ ./gradlew build --scan

BUILD SUCCESSFUL in 6s

Do you accept the Gradle Cloud Services license agreement (https://gradle.com/terms-of-service)? [yes, no]
yes
Gradle Cloud Services license agreement accepted.

Publishing build scan...
https://gradle.com/s/czajmbyg73t62
```

This mechanism makes it very easy to generate ad-hoc, one-off build scans without having to configure the build scan plugin in your build. If you need finer grained configuration, you can configure the build scan plugin in a build or init script as described in the following sections.

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



