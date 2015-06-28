# Task 规则

有时候也想要一个任务的行为是基于已经定义好的取值范围或者特定规则, 下面的例子就提供了一种很直观漂亮的方式:

**例子 15.25. 任务规则**

**build.gradle**

```
tasks.addRule("Pattern: ping<ID>") { String taskName ->
    if (taskName.startsWith("ping")) {
        task(taskName) << {
            println "Pinging: " + (taskName - 'ping')
        }
    }
}
```

**gradle -q pingServer1** 的输出

```
> gradle -q pingServer1
Pinging: Server1
```

这里的 String 参数就是用来定义规则的.

规则并不只是在通过命令行使用任务的时候执行. 你也可以基于规则来创建依赖关系:

**例子 15.26. 基于规则的任务依赖**

**build.gradle**

```
tasks.addRule("Pattern: ping<ID>") { String taskName ->
    if (taskName.startsWith("ping")) {
        task(taskName) << {
            println "Pinging: " + (taskName - 'ping')
        }
    }
}

task groupPing {
    dependsOn pingServer1, pingServer2
}
```

**gradle -q groupPing** 的输出

```
> gradle -q groupPing
Pinging: Server1
Pinging: Server2
```

如果你运行 “gradle -q tasks”, 你并不能找到名叫 “pingServer1” 或者 “pingServer2” 的任务, 但是这个脚本仍然会执行这些任务.
