# 构建Android应用

Android应用程序(也称为apps)通过唯一支持的IDE(集成开发环境)Android Studio采用Gradle作为编译工具。可以找到许多学习如何构建Android应用程序的资源。然而这篇用户手册，关注于创建新的Android应用程序时Gradle编译生成文件的细节，以及如何通过Gradle执行相关的构建任务(task)。

## [编译将产生的内容](#what_you_ll_build) {#what_you_ll_build}

你将创建一个“Hello,World!”Android应用程序，浏览其生成的Gradle构建文件以及执行一些常见任务(task)。

## [准备工作](#what_you_ll_need) {#what_you_ll_need}

* 26分钟左右

* version 2.4或更高版本的Android Studio。可以[下载](https://developer.android.com/studio/index.html) 带有最新版本Android SDK的Android Studio。集成开发环境(IDE)所需的系统配置要求详见上述下载网页

* version 1.7或更高版本的JDK

## [创建 Android Studio 项目](#create_a_new_android_studio_project) {#create_a_new_android_studio_project}

下载安装Android Studio后打开应用。在欢迎界面，如下图所示，点击标题链接 `Start a new Android Studio Project` (新建一个Android Studio项目)” 。当应用准备就绪，点击 `Next` 按钮。

![](https://guides.gradle.org/building-android-apps/images/Welcome-to-Android-Studio.png "Welcome to Android Studio")

在 `Create Android Project` (创建Android项目)页面，设置应用名( `Application name` )为 `HelloWorldGradle` ,设置公司域名(如附图中使用的是 `gradle.org` 域名)，并为项目选择方便的目录。然后点击 `Next` 按钮。

![](https://guides.gradle.org/building-android-apps/images/Create-New-Project.png "Create New Project")

在 `Target Android Devices` (在目标Android设备)页面，选中 `Phone and Tablet` (手机和平板电脑)并且从 `Minimum SDK` (最低SDK版本)下拉菜单中选择任意一个版本的API。如图选择的是比较通用的 `API 19`，并且选中的值对接下来的引导没有影响。

![](https://guides.gradle.org/building-android-apps/images/Target-Android-Devices.png "Target Android Devices")

在 `Add an Activity` (添加一个活动)页面，选中 `Empty Activity` (空的活动)然后点击 `Next` 按钮。

在 `Configure Activity` (配置活动)页面接收所有默认值，然后点击 `Finish` (完成)按钮。

![](https://guides.gradle.org/building-android-apps/images/Configure-Activity.png "Configure Activity")

## [查看生成的Gradle文件列表](#review_the_list_of_generated_gradle_files) {#review_the_list_of_generated_gradle_files}

如下图所示，Android Studio默认在 `Project View` (项目视图)中打开 `Android` 模式：

![](https://guides.gradle.org/building-android-apps/images/Project-View-Android.png "Project View Android")

Android项目是Gradle多项目构建，有一个顶级目录(根目录) `build.gradle` 文件和一个有自己 `build.gradle` 文件的 `app` 子目录。顶级目录构建文件在上图中标记为 `(Project:HelloWorldGradle)` ，`app` 构建文件是附加到它后面的 `Module:app` 。

项目里可能有两个名为 `gradle.properties` 的文件。一个是项目本地的，另一个是只有在电脑根目录的 `.gradle` 子目录中有一个全局的 `gradle.properties` 文件才存在的相同名称的可选文件。

`settings.gradle` 文件被Gradle用来配置多项目构建。它应该由如下线性结构组成：

```
include ':app'
```

这个设置告诉Gradle， `app` 子目录也是一个Gradle项目。如果在将来的时间里，你通过向导向该项目添加一个 `Android Library` (Android库)，另一个项目的子目录将会被创建并被添加到当前文件(夹)里。

最后一个文件是 `gradle-wrapper.properties` ,它配置了所谓的 `Gradle Wrapper` 。它允许你不需要现在Gradle就可以编译Android项目。该文件内容应该类似于：

```GROOVY
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-4.1-all.zip
```

前四行表明当wrapper第一次运行时，它会下载Gradle发行版本并将其存储在根目录的 `.gradle/wrapper/dists` 目录中。

最后一行显示了 `distributionUrl` (发行版本URL)的值，将要下载指定Gradle发行版本的下载地址。

> 项目具体版本号可能于此处显示的版本号(4.1)不同，并且URL可能引用到一个二进制版本( `bin` )而不是上述示例中的完整版本( `all` )。

## [查看顶级目录Gradle构建文件](#review_the_top_level_gradle_build_file) {#review_the_top_level_gradle_build_file}

项目里顶级目录 `build.gradle` 文件内容应该类似于：

```GROOVY
// 可以在顶级目录构建文件添加普适于所有子项目及模块的配置
// Top-level build file where you can add configuration options common to all sub-projects/modules.


buildscript { // 下载插件代码块

    repositories {
        google()
        jcenter()
    }
    dependencies { // 标识Android插件    

        classpath 'com.android.tools.build:gradle:3.0.1'
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects { // 所有项目(顶级和模块项目)的通用配置  

    repositories {
        google()
        jcenter()
    }
}

task clean(type: Delete) { // 一个名叫clean的task

    delete rootProject.buildDir
}
```

Gradle为构建文件定义了用于构建的领域特定语言(DSL)。`buildscript` 标签就是DSL中的一部分。它告诉Gradle编译需要一个可能不在标准Gradle发行版本中的插件，并且告诉Gradle在哪找到它。在上述示例中，所需的插件通过坐标语法“group:name:version”指定，示例中group(依赖分组，Maven中的 `groupId`)是 `com.android.tools.build` ,name(依赖名称，Maven中的artifactId)是 `gradle` ,version(依赖的版本，Maven中的version)是 `3.0.1` 。

> Gradle插件的版本号经常更新。请使用最新的插件，因为它将包含所有的bug修复和性能优化。

当Gradle第一次编译项目的时候，(Gradle)插件会下载下来并缓存到本地，因此这个下载任务只会执行一次。

`allprojects` 标签持有适用于顶级项目及其包含的任意子项目的配置细节。在上述示例中，代码块声明任何所需的依赖都应该从 `google` 或者 `jcenter` 中下载，其中 `jcenter` 的公共Bintray Artifactory仓储库链接为https://jcenter.bintray.com。

最后，编译的文件包含一个自定义的(或特定的)任务，任务名叫 `clean` 。它使用并通过配置内建的 `Delete` 任务实现删除项目根目录( `rootProject` )中的编译目录 ( `buildDir` )。两者都是项目属性，它们的默认值是该应用所在项目的构建( `build` )目录。

## [查看app模块下的构建文件](#review_the_build_file_in_the_app_module) {#review_the_build_file_in_the_app_module}

打开app模块下的 `buidl.gradle` 文件。第一行是：

```GROOVY
apply plugin: 'com.android.application'
```

它给当前项目运用了Android插件(在顶级目录构建文件中的 `buildscript` 部分提到的)。Gradle插件可以为Gradle项目添加自定义任务任务，新的配置项，依赖，以及其他的能力。在上述示例中，运用Android插件增加了一批任务，均配置在 `android` 代码块里，如下所示：

```GROOVY
android {
    compileSdkVersion 26

    defaultConfig {
        applicationId "org.gradle.helloworldgradle"
        minSdkVersion 19
        targetSdkVersion 26
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
```

这些属性同Gradle构建系统相比，与Android关联更大，因为它们只会在这里进行轻微的审查。简言之：

* 编译SDK版本号( `compileSdkVersion` )与Android SDK相关，并且应该保持最新可用的版本。

* 默认配置( `defaultConfig` )部分包含应用里所有变体(构建类型和产品风味的组合)共享的属性。

* 应用ID( `applicationId` ) 基于创建应用时指定的域名和项目名，并且该值在Google Play商店中必须是唯一的。

* 支持的最低SDK版本号( `minSdkVersion` )是应用将要支持的最低Android版本，目标SDK版本号( `targetSdkVersion` )应该是最新可用的Android版本。

* 版本号( `versionCode` )应该是一个整数，在将新版本上传到Google Play商店之前需要递增。版本号，和应用ID，告诉Google这是一个已有应用的更新版本，而不是一个新的应用。

* 版本名称( `versionName` )值用于项目内部的版本跟踪。

* `testInstrumentationRunner` 属性指定Android应用采用JUnit 4来测试。

在这部分的下面就是构建类型( `buildTypes` )。Android应用默认有两种构建类型： `debug` 和 `release` 。这个部分允许随意配置。 `debug` 部分并没有展示在上述示例中，这意味着 `debug` 所有设置均使用的默认值。

在 `android` 代码块后面，展示应用里所使用的库：

```
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:26.1.0'

    implementation 'com.android.support.constraint:constraint-layout:1.0.2'

    testImplementation 'junit:junit:4.12'

    androidTestImplementation 'com.android.support.test:runner:1.0.1'

    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'

}
```

配置依赖关系值构建Gradle应用程序的基础。在上述示例中， `dependencies` （依赖）部分展示了 `implementation` , `testImplementation` 以及 `androidTestImplementation` 部分配置的值。

先从最简单的开始说起， `testImplementation` 依赖项仅包含最新的JUnit 4稳定发行版本。JUnit类和测试的注解将在编译时在 `src/test/java` 层级中可用。

`androidTestImplementation` 依赖是指Espresso测试库，用于Android应用的集成测试。在上述示例中，Espresso没有通常包含的 `support-annotations` 库，因为通过其他依赖已经有了不同的版本( `support-annotations` 库)。在后面的步骤中，你讲看到如何找出依赖该库的版本号以及依赖关系。

最后，有三行向 `implementation` 添加依赖关系：

* 首先， `fileTree(dir: 'libs', include: ['*.jar'])` , 是一个文件树( `fileTree` )依赖，将 `libs` 文件里所有jar包添加到编译路径中去

* 其次， `com.android.support:appcompat-v7:26.1.0` 将Android兼容库添加到项目中。它允许你在SDK版本7及以后版本使用Material设计主题以及其他能力。

* 第三， `com.android.support.constraint:constraint-layout:1.0.2` 将Android约束布局( `Constraint Layout` )添加到项目中。它允许你在SDK9及以后版本中使用 `ConstraintLayout` 布局。

## [运行标准的Gradle任务](#run_standard_gradle_tasks) {#run_standard_gradle_tasks}

Android Studio通过集成开发环境(IDE)可以轻松构建和部署应用的调试版本，但最终Gradle仍然参与其中。要查看这些信息，打开Android Studio的终端窗口(或打开外部命令窗口并导航至应用所在根目录)。从那你能运行 `build` (编译)任务。

```
$ ./gradlew build
```

这行命令将要执行许多任务并最终返回"Build Successful"。要查看最终生成的APK(Android包，Android应用的可部署版本), 请查看 `app/build/outputs/apk` 目录。在那你将发现一个 `debug` 和一个 `release` 目录。`debug`目录包含 `app-debug.apk` ，是一个可以部署到模拟器或者已连接设备上的APK版本。如果你想部署一个release APK，首先需要创建一个签名，这部分超出了本指南的范围，但是按照资料描述来只是一个简单的流程。

从终端你还可以发现 `support-annotations` 模块已经被项目使用了。首先在 `app` 项目里运行 `dependencies` 任务，只查看 `releaseCompileClasspath` 配置的详细信息。
From the terminal, you can also find out the version of the`support-annotations`module being used in the project. To do so, first run the`dependencies`task in the`app`project, asking for the details of the`releaseCompileClasspath`configuration only.

```
$ ./gradlew :app:dependencies --configuration releaseCompileClasspath
:app:dependencies

------------------------------------------------------------
Project :app
------------------------------------------------------------

releaseCompileClasspath - Resolved configuration for compilation for variant: release
+--- com.android.support:appcompat-v7:26.1.0
|    +--- com.android.support:support-annotations:26.1.0
|    +--- com.android.support:support-v4:26.1.0
|    |    +--- com.android.support:support-compat:26.1.0
|    |    |    +--- com.android.support:support-annotations:26.1.0
|    |    |    \--- android.arch.lifecycle:runtime:1.0.0
|    |    |         +--- android.arch.lifecycle:common:1.0.0
|    |    |         \--- android.arch.core:common:1.0.0
|    |    +--- com.android.support:support-media-compat:26.1.0
|    |    |    +--- com.android.support:support-annotations:26.1.0
|    |    |    \--- com.android.support:support-compat:26.1.0 (*)
|    |    +--- com.android.support:support-core-utils:26.1.0
|    |    |    +--- com.android.support:support-annotations:26.1.0
|    |    |    \--- com.android.support:support-compat:26.1.0 (*)
|    |    +--- com.android.support:support-core-ui:26.1.0
|    |    |    +--- com.android.support:support-annotations:26.1.0
|    |    |    \--- com.android.support:support-compat:26.1.0 (*)
|    |    \--- com.android.support:support-fragment:26.1.0
|    |         +--- com.android.support:support-compat:26.1.0 (*)
|    |         +--- com.android.support:support-core-ui:26.1.0 (*)
|    |         \--- com.android.support:support-core-utils:26.1.0 (*)
|    +--- com.android.support:support-vector-drawable:26.1.0
|    |    +--- com.android.support:support-annotations:26.1.0
|    |    \--- com.android.support:support-compat:26.1.0 (*)
|    \--- com.android.support:animated-vector-drawable:26.1.0
|         +--- com.android.support:support-vector-drawable:26.1.0 (*)
|         \--- com.android.support:support-core-ui:26.1.0 (*)
\--- com.android.support.constraint:constraint-layout:1.0.2
     \--- com.android.support.constraint:constraint-layout-solver:1.0.2

(*) - dependencies omitted (listed previously)


BUILD SUCCESSFUL
```

从输出你能看见 `support-annotations` 模块，版本号是26.1.0，是 `appcompat-v7` 库的依赖。

另一个查看目标的版本号是运行 `dependencyInsight` 任务。执行如下命令(全部在一行)。

```
$ ./gradlew :app:dependencyInsight --dependency support-annotations --configuration releaseCompileClasspath
:app:dependencyInsight
com.android.support:support-annotations:26.1.0
+--- com.android.support:appcompat-v7:26.1.0
|    \--- releaseCompileClasspath
+--- com.android.support:support-compat:26.1.0
|    +--- com.android.support:support-vector-drawable:26.1.0
|    |    +--- com.android.support:appcompat-v7:26.1.0 (*)
|    |    \--- com.android.support:animated-vector-drawable:26.1.0
|    |         \--- com.android.support:appcompat-v7:26.1.0 (*)
|    +--- com.android.support:support-v4:26.1.0
|    |    \--- com.android.support:appcompat-v7:26.1.0 (*)
|    +--- com.android.support:support-media-compat:26.1.0
|    |    \--- com.android.support:support-v4:26.1.0 (*)
|    +--- com.android.support:support-fragment:26.1.0
|    |    \--- com.android.support:support-v4:26.1.0 (*)
|    +--- com.android.support:support-core-utils:26.1.0
|    |    +--- com.android.support:support-v4:26.1.0 (*)
|    |    \--- com.android.support:support-fragment:26.1.0 (*)
|    \--- com.android.support:support-core-ui:26.1.0
|         +--- com.android.support:animated-vector-drawable:26.1.0 (*)
|         +--- com.android.support:support-v4:26.1.0 (*)
|         \--- com.android.support:support-fragment:26.1.0 (*)
+--- com.android.support:support-core-ui:26.1.0 (*)
+--- com.android.support:support-core-utils:26.1.0 (*)
+--- com.android.support:support-media-compat:26.1.0 (*)
\--- com.android.support:support-vector-drawable:26.1.0 (*)

(*) - dependencies omitted (listed previously)


BUILD SUCCESSFUL
```

`dependency` 和 `dependencyInsight` 任务在任何Gradle项目中均可使用。它们能帮助你追踪并解决任何库版本冲突问题。

## [使用Gradle窗口](#use_the_gradle_window) {#use_the_gradle_window}

Android Studio有一个执行Gradle任务的特殊窗口。一个Android项目提供了超过80种不同的任务，这个窗口将它们分成不同的类别。

打开 `:app` 下的任务目录，查看 `android` 类别的内容。如下图所示。

![](https://guides.gradle.org/building-android-apps/images/Gradle-window-signingReport.png "Gradle window signingReport")

由于 `signingReport` 任务不需要任何参数，因此你只需要双击就可以执行它。结果如下图所示。

![](https://guides.gradle.org/building-android-apps/images/Run-and-Gradle-Console.png "Run and Gradle Console")

`signingReport` 任务告诉你公钥存储的目录(示例中 `debug.keystore` 在用户的根目录)，它的别称，以及它的MD5值和SHA1值。

请注意目前没有释放键。查看Gradle窗口中 `install` 类别下列出的任务，如下图所示。

![](https://guides.gradle.org/building-android-apps/images/Gradle-window-install.png "Gradle window install")

你会发现有一个 `installDebug` 任务和一个 `uninstallDebug` 任务，一个 `uninstallRelease` 任务，甚至还有一个 `uninstallAllTask` 任务。然而，可以发现还有一个缺少的 `installRelease` 任务。该任务( `installRelease` )仅在为release版本密钥创建签名配置后才可用。

如果你现在需要启动多个模拟器或者连接多台设备，则可以通过执行 `installDebug` 任务将应用部署到设备中。

```
$ ./gradlew installDebug
```

这与通过IDE运行应用程序不同。在这种情况下，你可以选择一个连接的设备或者模拟器，安装应用并启动它。Gradle中的 `installDebug` 任务将一步完成所有连接设备的应用部署工作，尽管它不会启动任何应用程序。结果与下图相似。

![](https://guides.gradle.org/building-android-apps/images/Android-Emulator-Pixel_API_25.png "Android Emulator Pixel API 25")

![](https://guides.gradle.org/building-android-apps/images/Android-Emulator-Nexus_9_API_23.png "Android Emulator Nexus 9 API 23")

像往常一样，你可以通过双击图标启动应用程序。你也可以通过 `uninstallAll` 任务卸载应用程序。

```
$ ./gradlew uninstallAll
```

它将从所有连接的设备中删除应用程序。

## [发布构建审视](#publish_a_build_scan) {#publish_a_build_scan}

[构建审视(Build scans)](https://gradle.com/build-scans) 是在构建时持久可共享的记录。通过构建审视，你可以对构建有更加深入的了解。

如果你使用的Gradle版本号高于4.3，你可以通过 `--scan` 命令行选项轻松创建一次构建审视，举个栗子， `gradle build --scan` 。对于老版本的Gradle，查看 [构建审视插件用户手册](https://docs.gradle.com/build-scan-plugin/#getting_set_up) 关于如何启用构件扫描。

```
./gradlew build --scan

BUILD SUCCESSFUL in 1s
4 actionable tasks: 4 executed

Do you accept the Gradle Cloud Services license agreement (https://gradle.com/terms-of-service)? [yes, no]
yes
Gradle Cloud Services license agreement accepted.

Publishing build scan...
https://gradle.com/s/carzirlfjwjlo
```

输出结果如下所示：

![](https://guides.gradle.org/building-android-apps/images/Build-scan-for-HelloWorldGradle.png "Build scan for HelloWorldGradle")

随意探索所有细节。该报告包含许多功能信息，包括依赖关系。如果你深入依赖部分并打开 `:app` 子项目的 `releaseCompileClasspath` 配置部分，在 `appcompat-v7` 库里面是前面描述过的 `support-annotations` 库。

![](https://guides.gradle.org/building-android-apps/images/Build-scan-dep-support-annotations.png "Build scan dep support annotations")

构建审视是分析构建的强有力方法。想查看更多细节，可以浏览 [构建审视入门指南](https://guides.gradle.org/creating-build-scans/) 和 [构建审视插件用户手册](https://docs.gradle.com/build-scan-plugin/)。

## [总结](#summary) {#summary}

在这篇手册中，你新建了一个Android应用程序并查看了许多Gradle功能。特别是，你学会了怎样来：

* 通过Android Studio创建一个Android应用

* 查看Gradle多项目构建的项目

* 往顶级Gradle构建文件中添加Android插件

* 在生成的wrapper中查看当前Gradle使用的版本号

* 阐释应用里 `android` 部分添加的设置

* 使用添加到应用程序的默认依赖

* 编译应用并查看生成的APK

* 确定依赖使用的具体版本号

* 通过Gradle窗口运行任务

* 多设备上的部署及卸载应用

* 在Android项目运行构建审视

## [下一步](#next_steps) {#next_steps}

由Ken Kousen编写并由O’Reilly Media, Inc出版的书籍《Gradle Recipes for Android》(《Android Gradle指南》)，已经在 [http://shop.oreilly.com/product/0636920032656.do](http://shop.oreilly.com/product/0636920032656.do) 开放购买了,但是电子版可以在 [https://gradle.org/books/](https://gradle.org/books/) 免费下载。

## [帮助改进本手册](#help_improve_this_guide) {#help_improve_this_guide}

有任何反馈或者疑问？发现错误？就像所有Gradle手册一样，帮助只需要Github上的一个issue。请提交一个issue或者提交PR到 [gradle-guides/building-android-apps](gradle-guides/building-android-apps) ,我们会尽快回复你。

