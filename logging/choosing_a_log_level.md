# Choosing a log level

你可以在命令行中选择如[表 17.2.Log 等级命令行选项]()所示的选项选择不同的日志级别.如[表 17.3.堆栈信息选项]()中所示的选项来选择堆栈信息.

**表17.2.Log 等级命令行选项**

| 选项 | 输出日志等级 |
| -- | -- |
| no logging options | LIFECYCLE及更高 |
| -q or --quiet | QUIET及更高 |
| -i or --info | INFO及更高 |
| -d or --debug | DEBUG及更高(所有日志信息) |


**表 17.3.堆栈信息选项**

| 选项 | 含义 |
| -- | -- |
| No stacktrace options | 无堆栈踪迹输出到控制台的情况下生成错误信息(如编译错误) ,仅在内部异常时打印堆栈信息.如果选择DEBUG日志等级,总会打印截断堆栈信息|
| -s or --stacktrace | 打印截断堆栈信息,我们推荐这个而不是`full stacktrace` ,Groovy的`full stacktrace`非常详细.(由于底层的动态调用机制。然而，他们通常不包含你的代码出了什么错的相关信息) |
| -S or --full-stacktrace | 打印全部堆栈信息 |


