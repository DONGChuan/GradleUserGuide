# Excluding tasks

You can exclude a task from being executed using the -x command-line option and providing the name of the task to exclude. Let's try this with the sample build file above.

Example 11.2. Excluding tasks

Output of gradle dist -x test

    > gradle dist -x test
    :compile
    compiling source
    :dist
    building the distribution

    BUILD SUCCESSFUL

    Total time: 1 secs

You can see from the output of this example, that the test task is not executed, even though it is a dependency of the dist task. You will also notice that the test task's dependencies, such as compileTest are not executed either. Those dependencies of test that are required by another task, such as compile, are still executed.


