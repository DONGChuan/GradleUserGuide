# Executing multiple tasks

You can execute multiple tasks in a single build by listing each of the tasks on the command-line. For example, the command gradle compile test will execute the compile and test tasks. Gradle will execute the tasks in the order that they are listed on the command-line, and will also execute the dependencies for each task. Each task is executed once only, regardless of how it came to be included in the build: whether it was specified on the command-line, or it a dependency of another task, or both. Let's look at an example.

Below four tasks are defined. Both dist and test depend on the compile task. Running gradle dist test for this build script results in the compile task being executed only once.

Figure 11.1. Task dependencies

Task dependencies
Example 11.1. Executing multiple tasks

build.gradle

    task compile << {
        println 'compiling source'
    }

    task compileTest(dependsOn: compile) << {
        println 'compiling unit tests'
    }

    task test(dependsOn: [compile, compileTest]) << {
        println 'running unit tests'
    }

    task dist(dependsOn: [compile, test]) << {
        println 'building the distribution'
    }

Output of gradle dist test

    > gradle dist test
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
Because each task is executed once only, executing gradle test test is exactly the same as executing gradle test.

