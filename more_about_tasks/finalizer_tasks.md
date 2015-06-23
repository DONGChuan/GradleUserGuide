# Finalizer tasks

Finalizers tasks are an incubating feature (see Section C.1.2, “Incubating”).
Finalizer tasks are automatically added to the task graph when the finalized task is scheduled to run.

Example 14.27. Adding a task finalizer

build.gradle
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}

taskX.finalizedBy taskY
Output of gradle -q taskX
> gradle -q taskX
taskX
taskY
Finalizer tasks will be executed even if the finalized task fails.

Example 14.28. Task finalizer for a failing task

build.gradle
task taskX << {
    println 'taskX'
    throw new RuntimeException()
}
task taskY << {
    println 'taskY'
}

taskX.finalizedBy taskY
Output of gradle -q taskX
> gradle -q taskX
taskX
taskY
On the other hand, finalizer tasks are not executed if the finalized task didn't do any work, for example if it is considered up to date or if a dependent task fails.

Finalizer tasks are useful in situations where the build creates a resource that has to be cleaned up regardless of the build failing or succeeding. An example of such a resource is a web container that is started before an integration test task and which should be always shut down, even if some of the tests fail.

To specify a finalizer task you use the Task.finalizedBy() method. This method accepts a task instance, a task name, or any other input accepted by Task.dependsOn().
