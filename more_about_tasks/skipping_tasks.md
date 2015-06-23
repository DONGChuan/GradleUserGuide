# Skipping tasks

Gradle offers multiple ways to skip the execution of a task.

14.8.1. Using a predicate

You can use the onlyIf() method to attach a predicate to a task. The task's actions are only executed if the predicate evaluates to true. You implement the predicate as a closure. The closure is passed the task as a parameter, and should return true if the task should execute and false if the task should be skipped. The predicate is evaluated just before the task is due to be executed.

Example 14.20. Skipping a task using a predicate

build.gradle
task hello << {
    println 'hello world'
}

hello.onlyIf { !project.hasProperty('skipHello') }
Output of gradle hello -PskipHello
> gradle hello -PskipHello
:hello SKIPPED

BUILD SUCCESSFUL

Total time: 1 secs
14.8.2. Using StopExecutionException

If the logic for skipping a task can't be expressed with a predicate, you can use the StopExecutionException. If this exception is thrown by an action, the further execution of this action as well as the execution of any following action of this task is skipped. The build continues with executing the next task.

Example 14.21. Skipping tasks with StopExecutionException

build.gradle
task compile << {
    println 'We are doing the compile.'
}

compile.doFirst {
    // Here you would put arbitrary conditions in real life.
    // But this is used in an integration test so we want defined behavior.
    if (true) { throw new StopExecutionException() }
}
task myTask(dependsOn: 'compile') << {
   println 'I am not affected'
}
Output of gradle -q myTask
> gradle -q myTask
I am not affected
This feature is helpful if you work with tasks provided by Gradle. It allows you to add conditional execution of the built-in actions of such a task. [6]

14.8.3. Enabling and disabling tasks

Every task has an enabled flag which defaults to true. Setting it to false prevents the execution of any of the task's actions.

Example 14.22. Enabling and disabling tasks

build.gradle
task disableMe << {
    println 'This should not be printed if the task is disabled.'
}
disableMe.enabled = false
Output of gradle disableMe
> gradle disableMe
:disableMe SKIPPED

BUILD SUCCESSFUL

Total time: 1 secs
