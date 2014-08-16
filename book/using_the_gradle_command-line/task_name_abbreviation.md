# Task name abbreviation

When you specify tasks on the command-line, you don't have to provide the full name of the task. You only need to provide enough of the task name to uniquely identify the task. For example, in the sample build above, you can execute task dist by running gradle d:

Example 11.3. Abbreviated task name

Output of gradle di

    > gradle di
    :compile
    compiling source
    :compileTest
    compiling unit tests
    :test
    running unit tests
    :dist
    building the distribution

    BUILD SUCCESSFUL

    Total time: 1 secs

You can also abbreviate each word in a camel case task name. For example, you can execute task compileTest by running gradle compTest or even gradle cT

Example 11.4. Abbreviated camel case task name

Output of gradle cT

    > gradle cT
    :compile
    compiling source
    :compileTest
    compiling unit tests

    BUILD SUCCESSFUL

    Total time: 1 secs

You can also use these abbreviations with the -x command-line option.

