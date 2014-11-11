# 获取依赖列表

执行 **gradle dependencies** 命令会列出项目的依赖列表, 所有依赖会根据任务区分,以树型结构展示出来.
如下例:

*例 10.13. 获取依赖信息*

*gradle -q dependencies api:dependencies webapp:dependencies 的输出结果*

    > gradle -q dependencies api:dependencies webapp:dependencies
    ------------------------------------------------------------
    Root project
    ------------------------------------------------------------

    No configurations

    ------------------------------------------------------------
    Project :api - The shared API for the application
    ------------------------------------------------------------

    compile
    \--- org.codehaus.groovy:groovy-all:2.3.3

    testCompile
    \--- junit:junit:4.11
         \--- org.hamcrest:hamcrest-core:1.3

    ------------------------------------------------------------
    Project :webapp - The Web application implementation
    ------------------------------------------------------------

    compile
    +--- project :api
    |    \--- org.codehaus.groovy:groovy-all:2.3.3
    \--- commons-io:commons-io:1.2

    testCompile
    No dependencies

虽然输出结果很多,
但这对于了解构建任务十分有用,
当然你可以通过 **--configuration** 参数来查看 指定构建任务的依赖情况:

*例 10.14. 过滤依赖信息*

*gradle -q api:dependencies --configuration testCompile 的输出结果*

    > gradle -q api:dependencies --configuration testCompile
    ------------------------------------------------------------
    Project :api - The shared API for the application
    ------------------------------------------------------------

    testCompile
    \--- junit:junit:4.11
         \--- org.hamcrest:hamcrest-core:1.3
