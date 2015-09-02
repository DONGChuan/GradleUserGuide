# 脚本插件

**Example 21.1. Applying a script plugin**

**build.gradle**

```gradle
apply from: 'other.gradle'
```

脚本插件可以从本地文件系统或在远程位置的脚本中*应用*.文件系统的位置是相对于项目目录,而远程脚本位置的是由一个`HTTP URL`指定的.多个脚本插件（两种形式之一）可以被*应用*到给定的构建。
