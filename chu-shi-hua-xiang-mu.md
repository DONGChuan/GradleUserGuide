# 初始化项目

First, let’s create a new directory where our project will go.

```
❯ mkdir basic-demo
❯ cd basic-demo
```

Now we can use Gradle’s`init`command to generate a simple project. We will explore everything that is generated so you know exactly what’s going on.

```
❯ gradle init
Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 3s
2 actionable tasks: 2 executed
```

The command should show "BUILD SUCCESSFUL" and generate the following "empty" project. If it doesn’t, please ensure that Gradle is[installed properly](https://gradle.org/install), and that you have the`JAVA_HOME`environment variable set correctly.

This is what Gradle generated for you.

```
.
├── build.gradle  

├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar  

│       └── gradle-wrapper.properties  

├── gradlew  

├── gradlew.bat  

└── settings.gradle  
```

|  | Project configuration script for configuring tasks in the current project |
| :--- | :--- |
|  | [Gradle Wrapper](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html)executable JAR |
|  | Gradle Wrapper configuration properties |
|  | Gradle Wrapper script for Unix-based systems |
|  | Gradle Wrapper script for Windows |
|  | Settings configuration script for configuring which Projects participate in the build |

|  | `gradle init`can generate[various different types of projects](https://docs.gradle.org/4.6/userguide/build_init_plugin.html#sec:build_init_types), and even knows how to translate simple`pom.xml`files to Gradle. |
| :--- | :--- |


Boom! Roasted. We could just end the guide here, but chances are you want to know how to_use_Gradle in this project. Let’s do that.

## [ ](https://guides.gradle.org/creating-new-gradle-builds/?_ga=2.191880558.599696663.1521685504-557532416.1521019880#create_a_task) {#create_a_task}



