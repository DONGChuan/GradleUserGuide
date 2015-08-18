### 22.13.1.执行测试
测试从main构建过程中分离出来的,运行在一个单独的JVM中执行.[Test](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.testing.Test.html)任务允许控制这些如何发生. 有许多属性用于控制测试过程如何启动.这包括使用诸如系统属性，JVM参数和Java可执行文件。

可以指定是否要并行执行测试.Gradle通过同时运行多个测试进程提供并行执行测试.每个测试进程在同一时间只能执行一个测试,为了充分利用这一特性,一般不需要为tests任务做什么特别的设置,`maxParallelForks`属性指定测试进程在同一时间运行的最大进程数.默认值是1,意味着不执行并行测试.

测试过程中设置org.gradle.test.worker系统属性为该测试过程的唯一标识符,例如，在文件名或其他资源标识符的唯一标识符。

你可以指定一些测试任务在已执行了一定数量的测试后重新运行.这可能是一个非常好的方式替代测试进程中的大量的堆.forkEvery属性指定测试类的在测试过程执行的最大数目。默认的是执行在各测设进程中不限数量的测试。

该任务有一个ignoreFailures属性来控制在测试失败时的行为。测试任务总是执行每一个检测试验.它停止构建之后，如果ignoreFailures是false，说明有失败的测试。ignoreFailures的默认值是false。

testLogging属性允许你配置哪个测试事件将被记录，并设置其log等级。默认情况下，将记录每一个失败的测试简明消息。详见[TestLoggingContainer](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.testing.logging.TestLoggingContainer.html)如何按需求调整测试记录。
