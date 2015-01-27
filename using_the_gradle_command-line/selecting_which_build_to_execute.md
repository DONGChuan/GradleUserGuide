# 选择文件构建

调用 gradle 命令时,
默认情况下总是会构建当前目录下的文件, 可以使用 **-b** 参数选择其他目录的构建文件,
并且当你使用此参数时 settings.gradle 将不会生效,
看下面的例子:

例 10.5. 选择文件构建

subdir/myproject.gradle

    task hello << {
        println "using build file '$buildFile.name' in '$buildFile.parentFile.name'."
    }

**gradle -q -b subdir/myproject.gradle hello** 的输出

    gradle -q -b subdir/myproject.gradle hello
    using build file 'myproject.gradle' in 'subdir'.

另外,你可以使用 **-p** 参数来指定构建的目录,例如在多项目构建中你可以用 -p 来替代 -b 参数

例 10.6. 选择构建目录

**gradle -q -p subdir hello** 命令的输出

    gradle -q -p subdir hello
    using build file 'build.gradle' in 'subdir'.

**-b** 参数用以指定脚本具体所在位置,
格式为 dirpwd/build.gradle.

**-p** 参数用以指定脚本目录即可.
