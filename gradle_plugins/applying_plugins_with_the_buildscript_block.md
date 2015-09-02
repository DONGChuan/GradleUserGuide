# 使用构建脚本块应用插件

项目可以通过添加向构建脚本中加入插件的类路径然后在应用插件,添加作为外部JAR文件的二进制插件.外部jar可以使用`buildscrip{}`块添加到构建脚本类路径就像[Section 62.6, “External dependencies for the build script”](https://docs.gradle.org/current/userguide/organizing_build_logic.html#sec:external_dependencies)中描述的一样.

**Example 21.4. Applying a plugin with the buildscript block**

**build.gradle**

```gradle
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath "com.jfrog.bintray.gradle:gradle-bintray-plugin:0.4.1"
    }
}

apply plugin: "com.jfrog.bintray"
```


