# 创建 Task

Gradle provides APIs for creating and configuring tasks through a Groovy or Kotlin-based DSL. A[`Project`](https://docs.gradle.org/4.6/dsl/org.gradle.api.Project.html)includes a collection of[`Task`](https://docs.gradle.org/4.6/dsl/org.gradle.api.Task.html)s, each of which performs some basic operation.

Gradle comes with a library of tasks that you can configure in your own projects. For example, there is a core type called`Copy`, which copies files from one location to another. The`Copy`task is very useful \([see the documentation](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.Copy.html)for details\), but here, once again, let’s keep it simple. Perform the following steps:

1. Create a directory called`src`.

2. Add a file called`myfile.txt`in the`src`directory. The contents are arbitrary \(it can even be empty\), but for convenience add the single line`Hello, World!`to it.

3. Define a task called`copy`of type`Copy`\(note the capital letter\) in the main build file,`build.gradle`, that copies the`src`directory to a new directory called`dest`. \(You don’t have to create the`dest`directory — the task will do it for you.\)

```
task copy(
type
: Copy, 
group
: 
"
Custom
"
, 
description
: 
"
Copies sources to the dest directory
"
) {
    from 
"
src
"

    into 
"
dest
"

}
```

Here,`group`and`description`can be anything you want. You can even omit them, but doing so will also omit them from the`tasks`report, used later.

Now execute your new`copy`task:

```
❯ ./gradlew copy
:copy

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```

Verify that it worked as expected by checking that there is now a file called`myfile.txt`in the`dest`directory, and that its contents match the contents of the same one in the`src`directory.



