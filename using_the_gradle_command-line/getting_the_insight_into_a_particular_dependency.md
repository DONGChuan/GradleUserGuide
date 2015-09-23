# 查看特定依赖

执行 `gradle dependencyInsight` 命令可以查看指定的依赖. 如下面的例子.

**例 11.15. 获取特定依赖**

`gradle -q webapp:dependencyInsight --dependency groovy --configuration compile` 的输出结果

```
> gradle -q webapp:dependencyInsight --dependency groovy --configuration compile
org.codehaus.groovy:groovy-all:2.3.3
\--- project :api
     \--- compile
```


这个 task 对于了解依赖关系、了解为何选择此版本作为依赖十分有用.了解更多请参阅 [DependencyInsightReportTask](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.diagnostics.DependencyInsightReportTask.html).

dependencyInsight 任务是'Help'任务组中的一个. 这项任务需要进行配置才可以使用. Report 查找那些在指定的配置里特定的依赖.

如果用了Java相关的插件, 那么 dependencyInsight 任务已经预先被配置到'compile'下了. 你只需要通过 `'--dependency'` 参数来制定所需查看的依赖即可. 如果你不想用默认配置的参数项你可以通过`'--configuration'`参数来进行指定.了解更多请参阅 [DependencyInsightReportTask](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.diagnostics.DependencyInsightReportTask.html).
