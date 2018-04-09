# 构建Android应用

Android applications \(known colloquially as_apps_\) use Gradle as their build tool, normally through the only supported IDE, Android Studio. Many resources are available for learning how to build Android applications. This guide, however, focuses on the details of the Gradle build files generated when creating a new Android application, and how to use Gradle to invoke relevant build tasks for them.

Contents

* [What you’ll build](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#what_you_ll_build)
* [What you’ll need](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#what_you_ll_need)
* [Create a new Android Studio project](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#create_a_new_android_studio_project)
* [Review the list of generated Gradle files](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#review_the_list_of_generated_gradle_files)
* [Review the top-level Gradle build file](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#review_the_top_level_gradle_build_file)
* [Review the build file in the app module](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#review_the_build_file_in_the_app_module)
* [Run standard Gradle tasks](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#run_standard_gradle_tasks)
* [Use the Gradle window](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#use_the_gradle_window)
* [Publish a build scan](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#publish_a_build_scan)
* [Summary](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#summary)
* [Next Steps](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#next_steps)
* [Help improve this guide](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#help_improve_this_guide)

## [What you’ll build](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#what_you_ll_build) {#what_you_ll_build}

You’ll create a "Hello, World!" type of Android application, explore its generated Gradle build files, and execute common tasks using them.

## [What you’ll need](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#what_you_ll_need) {#what_you_ll_need}

* About26 minutes

* Android Studio, version 2.4 or higher. You can download Android Studio, along with the latest Android SDK, from[https://developer.android.com/studio/index.html](https://developer.android.com/studio/index.html). The system requirements for the IDE are also found at that link.

* The Java Development Kit \(JDK\), version 1.7 or higher

## [Create a new Android Studio project](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#create_a_new_android_studio_project) {#create_a_new_android_studio_project}

After downloading and installing Android Studio, start the application. On the welcome screen, click the link entitled, "Start a new Android Studio project", as shown in the figure. When ready, click_Next_.

![](https://guides.gradle.org/building-android-apps/images/Welcome-to-Android-Studio.png "Welcome to Android Studio")

On the "Create Android Project" screen, set the application name to "HelloWorldGradle", the company domain to your own \(the domain_gradle.org\_is used in the accompanying figure\), and select any convenient directory for the project location. Then click\_Next_.

![](https://guides.gradle.org/building-android-apps/images/Create-New-Project.png "Create New Project")

On the "Target Android Devices" screen, select\_Phone and Tablet\_and choose any recent API level from the\_Minimum SDK\_drop-down list. The figure shows API 19, which is common, but the value chose will not affect the rest of this guide.

![](https://guides.gradle.org/building-android-apps/images/Target-Android-Devices.png "Target Android Devices")

On the "Add an Activity" screen, select_Empty Activity\_and click\_Next_.

Accept all the defaults on the "Configure Activity" screen and click_Finish_.

![](https://guides.gradle.org/building-android-apps/images/Configure-Activity.png "Configure Activity")

## [Review the list of generated Gradle files](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#review_the_list_of_generated_gradle_files) {#review_the_list_of_generated_gradle_files}

By default, Android Studio will start with a "Project View" in "Android" mode, as shown in the figure:

![](https://guides.gradle.org/building-android-apps/images/Project-View-Android.png "Project View Android")

Android projects are Gradle multi-project builds, with a top-level`build.gradle`file and a subdirectory called`app`, with its own`build.gradle`file. The top-level build file is noted as`(Project: HelloWorldGradle)`in the figure, and the`app`build file has`(Module: app)`appended to it.

There may be two files called`gradle.properties`. One is local to the project. The other, optional file of the same name exists only if you have a global`gradle.properties`file in the`.gradle`sub-directory of your home directory.

The file`settings.gradle`is used by Gradle to configure the multi-project build. It should consist of a single line:

```
include 
'
:app
'
```

This tells Gradle that the`app`sub-directory is also a Gradle project. If, at some later time, you were to add an Android Library to this project through the available wizard, another project sub-directory would be created and added to this file.

The last file is called`gradle-wrapper.properties`, which configures the so-called Gradle Wrapper. This allows you to build Android projects without having to install Gradle first. The contents of the file should be similar to:

```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https
\
:
//services.gradle.org/distributions/gradle-4.1-all.zip
```

The first four lines indicate that when the wrapper runs the first time, it will download a Gradle distribution and store it in the directory`.gradle/wrapper/dists`in your home directory.

The last line shows the value of the`distributionUrl`, which is the location where Gradle will download the distribution specified.

|  | The specific version number might differ from that shown here \(4.1\), and the URL might refer to a binary version \(`-bin`\) instead of the complete \(`-all`\) version shown in this example. |
| :--- | :--- |


## [Review the top-level Gradle build file](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#review_the_top_level_gradle_build_file) {#review_the_top_level_gradle_build_file}

The project`build.gradle`file should have contents similar to:

```
// Top-level build file where you can add configuration options common to all sub-projects/modules.


buildscript {                 

    repositories {
        google()
        jcenter()
    }
    dependencies {            

        classpath 
'
com.android.tools.build:gradle:3.0.1
'
// NOTE: Do not place your application dependencies here; they belong
// in the individual module build.gradle files

    }
}

allprojects {                 

    repositories {
        google()
        jcenter()
    }
}

task clean(
type
: Delete) {    

    delete rootProject.buildDir
}
```

|  | Download plugins block |
| :--- | :--- |
|  | Identifies the Android plugin |
|  | Configuration for top-level and module projects |
|  | Ad hoc task |

Gradle defines a domain-specific language \(DSL\) for builds, used inside the build files. The`buildscript`tag is part of that DSL. It tells Gradle that the build requires a plugin that may not be part of the baseline Gradle distribution, and tells Gradle where to find it. In this case, the required plugin is specified using coordinate syntax "group:name:version", where the group is`com.android.tools.build`, the name is`gradle`, and the version is`3.0.1`.

|  | The version number of the Gradle plugin is updated frequently. Please use the latest plugin, as it will contain all the available bug fixes and performance improvements. |
| :--- | :--- |


When Gradle builds this project the first time, the plugin will be downloaded and cached, so this task is only performed once.

The`allprojects`tag holds configuration details that apply to both the top-level project and any sub-projects it contains. In this case, the block specifies that any required dependencies should be downloaded from`google`, or`jcenter`, the public Bintray Artifactory repository at[https://jcenter.bintray.com](https://jcenter.bintray.com/).

Finally, the build file contains a custom \(or_ad hoc_\) task, called`clean`. It uses the built-in task type`Delete`and configures it so that the`clean`task will delete the`buildDir`in the`rootProject`. Both are project properties, whose values default to the`build`directory in the project where this app resides.

## [Review the build file in the app module](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#review_the_build_file_in_the_app_module) {#review_the_build_file_in_the_app_module}

Open the`build.gradle`file in the`app`module. The first line is:

```
apply 
plugin
: 
'
com.android.application
'
```

This "applies" the Android plugin \(referred to in the`buildscript`section of the top-level build file\) to the current project. Plugins in Gradle can add custom tasks, new configurations, dependencies, and other capabilities to Gradle projects. In this case, applying the Android plugin adds a wide variety of tasks, which are configured by the`android`block shown next.

```
android {
    compileSdkVersion 
26

    defaultConfig {
        applicationId 
"
org.gradle.helloworldgradle
"

        minSdkVersion 
19

        targetSdkVersion 
26

        versionCode 
1

        versionName 
"
1.0
"

        testInstrumentationRunner 
"
android.support.test.runner.AndroidJUnitRunner
"

    }
    buildTypes {
        release {
            minifyEnabled 
false

            proguardFiles getDefaultProguardFile(
'
proguard-android.txt
'
), 
'
proguard-rules.pro
'

        }
    }
}
```

These properties are more relevant to Android than the Gradle build system, to they will only be lightly reviewed here. In short:

* The`compileSdkVersion`is associated with the Android SDK and should always be the latest available version.

* The`defaultConfig`section hold properties that are shared by all variants \(combinations of build types and product flavors\) of the app.

* The`applicationId`is based on the domain name and project name specified when creating the app, and must be unique in the Google Play store.

* The value of`minSdkVersion`is the minimum Android API you are willing to support with this app, and the`targetSdkVersion`should be the latest Android version available.

* The value of`versionCode`should be an integer that is incremented before uploading a new version of the app into the Google Play store. This value, along with the`applicationId`, tell Google that this is a new version of an existing app, as opposed to a new app.

* The`versionName`value is used for your own internal version tracking.

* The`testInstrumentationRunner`property is configured to use the JUnit 4 test runner configured for Android apps.

Below this section is a block called`buildTypes`. By default, Android apps support two build types,`debug`and`release`. This section allows you to configure each however you like. The`debug`section is not shown here, implying that all the default settings for`debug`are being used.

After the`android`block, there is a block that shows the libraries used for this app.

```
dependencies {
    implementation fileTree(
dir
: 
'
libs
'
, 
include
: [
'
*.jar
'
])
    implementation 
'
com.android.support:appcompat-v7:26.1.0
'

    implementation 
'
com.android.support.constraint:constraint-layout:1.0.2
'

    testImplementation 
'
junit:junit:4.12
'

    androidTestImplementation 
'
com.android.support.test:runner:1.0.1
'

    androidTestImplementation 
'
com.android.support.test.espresso:espresso-core:3.0.1
'

}
```

Configuring dependencies is a fundamental part of building Gradle applications. In this case, the`dependencies`section shows values for the`implementation`,`testImplementation`, and`androidTestImplementation`configurations.

Taking the simplest one first, the`testImplementation`dependency consists only of the latest stable JUnit 4 distribution. The JUnit classes and test annotations will then be available at compile time in the`src/test/java`hierarchy.

The`androidTestImplementation`dependency refers to the Espresso testing library, which is used for integration testing of Android apps. In this case, Espresso is requested without the`support-annotations`library that it would normally include, because a different version is already included through other dependencies. In a later step, you’ll see how to find out what version of this library was included and why.

Finally, there are three lines that add dependencies to the`implementation`configuration:

* The first,`fileTree(dir: 'libs', include: ['*.jar'])`, is a`fileTree`dependency that adds any jar files in the`libs`folder to the compile classpath

* The second,`com.android.support:appcompat-v7:26.1.0`adds the Android Compatibility library to the project. This allows you to use the Material design theme and other capabilities in any Android app as old as SDK version 7.

* The third,`com.android.support.constraint:constraint-layout:1.0.2`adds the Android Constraint Layout to the project. This allows you to use the ConstraintLayout layout class in any Android app as old as SDK version 9.

## [Run standard Gradle tasks](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#run_standard_gradle_tasks) {#run_standard_gradle_tasks}

Android Studio makes it easy to build and deploy a debug version of an app through the IDE, but ultimately Gradle is still involved. To see this, open the Terminal window in Android Studio \(or open an external command prompt and navigate to the root directory of your app\). From there you can run the`build`task.

```
$ ./gradlew build
```

This will run many tasks and eventually return "Build Successful". To see the resulting APK \(Android package, the deployable version of an Android app\), look in the directory`app/build/outputs/apk`. There you will find a`debug`and a`release`directories. The`debug`directory contains`app-debug.apk`, which is the APK version that will be deployed to an emulator or connected device. If you want to deploy a release APK, you need to create a signing configuration first, which is beyond the scope of this guide, but is a straightforward process described in the resources.

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

From the output, you can see that the`support-annotations`module, version 26.1.0, is a dependency of the`appcompat-v7`library.

Another way to see the version required is to use the`dependencyInsight`task. Run the following command \(all on one line\).

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

Both the`dependency`and`dependencyInsight`tasks are available in any Gradle project. They can help you track down and resolve any issues with library version conflicts.

## [Use the Gradle window](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#use_the_gradle_window) {#use_the_gradle_window}

Android Studio includes a special window for executing Gradle tasks. Android projects provide over 80 different tasks, and this window tries to organize them into categories.

Open the Tasks folder under`:app`, and look inside the`android`category. The following figure shows an example.

![](https://guides.gradle.org/building-android-apps/images/Gradle-window-signingReport.png "Gradle window signingReport")

Since the`signingReport`task does not require any arguments, you can simply double-click on it to execute it. The results are shown in the next figure.

![](https://guides.gradle.org/building-android-apps/images/Run-and-Gradle-Console.png "Run and Gradle Console")

The`signingReport`task tells you where the public key is stored \(here the`debug.keystore`file in the user’s root directory\), its alias, and its MD5 and SHA1 hashes.

Note that there is no release key at the moment. Look at the tasks listed in the Gradle window in the`install`category, as shown the next figure.

![](https://guides.gradle.org/building-android-apps/images/Gradle-window-install.png "Gradle window install")

You’ll see that there is an`installDebug`task and an`uninstallDebug`task, an`uninstallRelease`task, and even an`uninstallAllTask`. Conspicuous by its absence, however, is an`installRelease`task. That task is only available if you create a signing configuration for a release key, which Gradle can use to create a signed release APK.

If you were now to start up multiple emulators or attach multiple devices, you could deploy the app into all of them by executing the`installDebug`task.

```
$ ./gradlew installDebug
```

This is different from running the app through the IDE. In that case, you would select a single connected device or emulator, and would both install the app and start it up. The`installDebug`task from Gradle will deploy the app in all connected devices in one step, though it will not start the app in any of them. The result will be similar to the next figure.

![](https://guides.gradle.org/building-android-apps/images/Android-Emulator-Pixel_API_25.png "Android Emulator Pixel API 25")

![](https://guides.gradle.org/building-android-apps/images/Android-Emulator-Nexus_9_API_23.png "Android Emulator Nexus 9 API 23")

You can launch the app by double-clicking on the icon, as usual. You can also remove the app by using the`uninstallAll`task.

```
$ ./gradlew uninstallAll
```

This will remove the app from all connected devices.

## [Publish a build scan](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#publish_a_build_scan) {#publish_a_build_scan}

[Build scans](https://gradle.com/build-scans)are a persistent, shareable record of what happened when running a build. With build scans, you gain deep insights about your build.

If you are using Gradle 4.3+, you can easily create a build scan by using the`--scan`command line option, e.g.`gradle build --scan`. For older Gradle versions, see the[Build Scan Plugin User Manual](https://docs.gradle.com/build-scan-plugin/#getting_set_up)on how to enable build scans.

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

The resulting page will resemble:

![](https://guides.gradle.org/building-android-apps/images/Build-scan-for-HelloWorldGradle.png "Build scan for HelloWorldGradle")

Feel free to explore all the details. The report contains information on many features, including dependencies. If you dig into the dependencies section and open the`releaseCompileClasspath`configuration of the`:app`subproject, inside the`appcompat-v7`library is the`support-annotations`library described earlier.

![](https://guides.gradle.org/building-android-apps/images/Build-scan-dep-support-annotations.png "Build scan dep support annotations")

Build scans are a powerful way to analyze your build. For more details, see the[Getting Started guide for Creating Build Scans](https://guides.gradle.org/creating-build-scans/)and the[Build Scan Plugin User Manual](https://docs.gradle.com/build-scan-plugin/).

## [Summary](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#summary) {#summary}

In this guide, you created an Android app and examined many of the Gradle capabilities that came with it. Specifically, you learned how to:

* Create an Android app using Android Studio

* View the generated project as a Gradle multi-project build

* Add the Android plugin to the top-level Gradle build file

* See which version of Gradle is used in the generated wrapper

* Interpret the settings added in the`android`section of the app

* Work with the default dependencies added to the app

* Build the app and see the output APK

* Determine which version of a dependency is being used

* Use the Gradle window to execute tasks

* Deploy and uninstall the app on multiple devices

* Run a build scan on an Android project

## [Next Steps](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#next_steps) {#next_steps}

The book_Gradle Recipes for Android_, by Ken Kousen and published by O’Reilly Media, Inc, is available for purchase at[http://shop.oreilly.com/product/0636920032656.do](http://shop.oreilly.com/product/0636920032656.do), but an electronic version can also be downloaded for free at[https://gradle.org/books/](https://gradle.org/books/).

## [Help improve this guide](https://guides.gradle.org/building-android-apps/?_ga=2.62770159.216844200.1523258211-2050954084.1517482353#help_improve_this_guide) {#help_improve_this_guide}

Have feedback or a question? Found a typo? Like all Gradle guides, help is just a GitHub issue away. Please add an issue or pull request to[gradle-guides/building-android-apps](https://github.com/gradle-guides/building-android-apps/)and we’ll get back to you.

