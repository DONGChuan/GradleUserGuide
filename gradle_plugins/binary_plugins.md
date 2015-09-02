# 二进制插件

**Example 21.2. Applying a binary plugin**

**build.gradle**
```gradle
apply plugin: 'java'
```

插件可以使用*插件ID*应用.插件的id作为给定的插件的唯一标识符.核心插件注册一个可以用作插件的id的短名称.在上述情况下,我们可以使用简称`java`的插件以应用[JavaPlugin](https://docs.gradle.org/current/javadoc/org/gradle/api/plugins/JavaPlugin.html).社区插件,一方面会使用一个完全合格的形式的插件id(如`com.github.foo.bar`),但还是有一些传统的插件可能仍然使用很短的，不合格的格式.

不使用一个插件的id，插件也可以通过简单地指定类来应用插件：

**Example 21.3. Applying a binary plugin by type**

**build.gradle**

```gradle
apply plugin: JavaPlugin
```

在上面的例子中,JavaPlugin是指[JavaPlugin](https://docs.gradle.org/current/javadoc/org/gradle/api/plugins/JavaPlugin.html),此类不是严格需要导入org.gradle.api.plugins包中的所有自动导入构建脚本（见:[附录E，现有的IDE支持，以及如何没有它应付](https://docs.gradle.org/current/userguide/ide_support.html)).此外，这是没有必要追加的.class以识别一个类常量在Groovy，因为它是在Java中。

