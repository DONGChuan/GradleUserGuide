# Environment 选项

Gradle 的许多方面都可以通过以下的选项被自定义, 比如构建脚本的位置, 设置, 缓存等等. 你可以通过学习[构建环境](https://docs.gradle.org/current/userguide/build_environment.html)来自定义你的构建.

`-b`,`--build-file`

指定构建文件. 比如:`gradle --build-file=foo.gradle`, 因为默认的文件是`build.gradle`, 如果没找到, 将依次寻找`build.gradle.kts` 和 `myProjectName.gradle`. 通过使用这个选项, Gradle 将直接查找 foo.gradle 文件作为构建文件.

`-c`,`--settings-file`

指定设置文件. 比如:`gradle --settings-file=somewhere/else/settings.gradle`

`-g`,`--gradle-user-home`

指定 Gradle 默认的用户 home 路径. 默认是用户 home 目录下的 `.gradle` 文件目录.

`-p`,`--project-dir`

指定 Gradle 开始构建的文件夹, 或者说是需要构建的项目所在的文件夹, 默认是当前文件夹.

`--project-cache-dir`

指定项目相关的缓存文件夹. 默认是项目根目录的 `.gradle` 文件夹.

`-u`,`--no-search-upward`\(deprecated\)

不要去父文件夹寻找 `settings.gradle` 文件.

`-D`,`--system-prop`

设置 JVM 的一个系统属性, 比如 `-Dmyprop=myvalue`. 查看[系统属性](https://docs.gradle.org/current/userguide/build_environment.html#sec:gradle_system_properties)了解更多.

`-I`,`--init-script`

指定初始化脚本. 查看[初始化脚本](https://docs.gradle.org/current/userguide/init_scripts.html)了解更多.

`-P`,`--project-prop`

设置根项目的一个项目属性，比如 `-Pmyprop=myvalue`. 查看[项目属性](https://docs.gradle.org/current/userguide/build_environment.html#sec:project_properties)了解更多.

`-Dorg.gradle.jvmargs`

设置 JVM 参数.

`-Dorg.gradle.java.home`

设置 JDK home 目录.

