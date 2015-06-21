# Ordering tasks

Task ordering is an incubating feature. Please be aware that this feature may change in later Gradle versions.
In some cases it is useful to control the order in which 2 tasks will execute, without introducing an explicit dependency between those tasks. The primary difference between a task ordering and a task dependency is that an ordering rule does not influence which tasks will be executed, only the order in which they will be executed.

Task ordering can be useful in a number of scenarios:

Enforce sequential ordering of tasks: eg. 'build' never runs before 'clean'.
Run build validations early in the build: eg. validate I have the correct credentials before starting the work for a release build.
Get feedback faster by running quick verification tasks before long verification tasks: eg. unit tests should run before integration tests.
A task that aggregates the results of all tasks of a particular type: eg. test report task combines the outputs of all executed test tasks.
There are two ordering rules available: “must run after” and “should run after”.

When you use the “must run after” ordering rule you specify that taskB must always run after taskA, whenever both taskA and taskB will be run. This is expressed as taskB.mustRunAfter(taskA). The “should run after” ordering rule is similar but less strict as it will be ignored in two situations. Firstly if using that rule introduces an ordering cycle. Secondly when using parallel execution and all dependencies of a task have been satisfied apart from the “should run after” task, then this task will be run regardless of whether its “should run after” dependencies have been run or not. You should use “should run after” where the ordering is helpful but not strictly required.

With these rules present it is still possible to execute taskA without taskB and vice-versa.

Example 14.14. Adding a 'must run after' task ordering

build.gradle
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}
taskY.mustRunAfter taskX
Output of gradle -q taskY taskX
> gradle -q taskY taskX
taskX
taskY
Example 14.15. Adding a 'should run after' task ordering

build.gradle
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}
taskY.shouldRunAfter taskX
Output of gradle -q taskY taskX
> gradle -q taskY taskX
taskX
taskY
In the examples above, it is still possible to execute taskY without causing taskX to run:

Example 14.16. Task ordering does not imply task execution

Output of gradle -q taskY
> gradle -q taskY
taskY
To specify a “must run after” or “should run after” ordering between 2 tasks, you use the Task.mustRunAfter() and Task.shouldRunAfter() methods. These methods accept a task instance, a task name or any other input accepted by Task.dependsOn().

Note that “B.mustRunAfter(A)” or “B.shouldRunAfter(A)” does not imply any execution dependency between the tasks:

It is possible to execute tasks A and B independently. The ordering rule only has an effect when both tasks are scheduled for execution.
When run with --continue, it is possible for B to execute in the event that A fails.
As mentioned before, the “should run after” ordering rule will be ignored if it introduces an ordering cycle:

Example 14.17. A 'should run after' task ordering is ignored if it introduces an ordering cycle

build.gradle
task taskX << {
    println 'taskX'
}
task taskY << {
    println 'taskY'
}
task taskZ << {
    println 'taskZ'
}
taskX.dependsOn taskY
taskY.dependsOn taskZ
taskZ.shouldRunAfter taskX
Output of gradle -q taskX
> gradle -q taskX
taskZ
taskY
taskX
