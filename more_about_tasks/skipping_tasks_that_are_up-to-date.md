# Skipping tasks that are up-to-date

If you are using one of the tasks that come with Gradle, such as a task added by the Java plugin, you might have noticed that Gradle will skip tasks that are up-to-date. This behaviour is also available for your tasks, not just for built-in tasks.

14.9.1. Declaring a task's inputs and outputs

Let's have a look at an example. Here our task generates several output files from a source XML file. Let's run it a couple of times.

Example 14.23. A generator task

build.gradle
task transform {
    ext.srcFile = file('mountains.xml')
    ext.destDir = new File(buildDir, 'generated')
    doLast {
        println "Transforming source file."
        destDir.mkdirs()
        def mountains = new XmlParser().parse(srcFile)
        mountains.mountain.each { mountain ->
            def name = mountain.name[0].text()
            def height = mountain.height[0].text()
            def destFile = new File(destDir, "${name}.txt")
            destFile.text = "$name -> ${height}\n"
        }
    }
}
Output of gradle transform
> gradle transform
:transform
Transforming source file.
Output of gradle transform
> gradle transform
:transform
Transforming source file.
Notice that Gradle executes this task a second time, and does not skip the task even though nothing has changed. Our example task was defined using an action closure. Gradle has no idea what the closure does and cannot automatically figure out whether the task is up-to-date or not. To use Gradle's up-to-date checking, you need to declare the inputs and outputs of the task.

Each task has an inputs and outputs property, which you use to declare the inputs and outputs of the task. Below, we have changed our example to declare that it takes the source XML file as an input and produces output to a destination directory. Let's run it a couple of times.

Example 14.24. Declaring the inputs and outputs of a task

build.gradle
task transform {
    ext.srcFile = file('mountains.xml')
    ext.destDir = new File(buildDir, 'generated')
    inputs.file srcFile
    outputs.dir destDir
    doLast {
        println "Transforming source file."
        destDir.mkdirs()
        def mountains = new XmlParser().parse(srcFile)
        mountains.mountain.each { mountain ->
            def name = mountain.name[0].text()
            def height = mountain.height[0].text()
            def destFile = new File(destDir, "${name}.txt")
            destFile.text = "$name -> ${height}\n"
        }
    }
}
Output of gradle transform
> gradle transform
:transform
Transforming source file.
Output of gradle transform
> gradle transform
:transform UP-TO-DATE
Now, Gradle knows which files to check to determine whether the task is up-to-date or not.

The task's inputs property is of type TaskInputs. The task's outputs property is of type TaskOutputs.

A task with no defined outputs will never be considered up-to-date. For scenarios where the outputs of a task are not files, or for more complex scenarios, the TaskOutputs.upToDateWhen() method allows you to calculate programmatically if the tasks outputs should be considered up to date.

A task with only outputs defined will be considered up-to-date if those outputs are unchanged since the previous build.

14.9.2. How does it work?

Before a task is executed for the first time, Gradle takes a snapshot of the inputs. This snapshot contains the set of input files and a hash of the contents of each file. Gradle then executes the task. If the task completes successfully, Gradle takes a snapshot of the outputs. This snapshot contains the set of output files and a hash of the contents of each file. Gradle persists both snapshots for the next time the task is executed.

Each time after that, before the task is executed, Gradle takes a new snapshot of the inputs and outputs. If the new snapshots are the same as the previous snapshots, Gradle assumes that the outputs are up to date and skips the task. If they are not the same, Gradle executes the task. Gradle persists both snapshots for the next time the task is executed.

Note that if a task has an output directory specified, any files added to that directory since the last time it was executed are ignored and will NOT cause the task to be out of date. This is so unrelated tasks may share an output directory without interfering with each other. If this is not the behaviour you want for some reason, consider using TaskOutputs.upToDateWhen()


