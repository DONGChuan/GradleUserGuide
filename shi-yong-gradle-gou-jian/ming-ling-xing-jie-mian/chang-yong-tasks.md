# 常用 Tasks

The following are task conventions applied by built-in and most major Gradle plugins.

### Computing all outputs

It is common in Gradle builds for the`build`task to designate assembling all outputs and running all checks.

```
❯ gradle build
```

### Running applications

It is common for applications to be run with the`run`task, which assembles the application and executes some script or binary.

```
❯ gradle run
```

### Running all checks

It is common for_all_verification tasks, including tests and linting, to be executed using the`check`task.

```
❯ gradle check
```

### Cleaning outputs

You can delete the contents of the build directory using the`clean`task, though doing so will cause pre-computed outputs to be lost, causing significant additional build time for the subsequent task execution.

```
❯ gradle clean
```



