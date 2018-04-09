Continuous Build allows you to automatically re-execute the requested tasks when task inputs change.

For example, you can continuously run the`test`task and all dependent tasks by running:

```
❯ gradle test --continuous
```

Gradle will behave as if you ran`gradle test`after a change to sources or tests that contribute to the requested tasks. This means that unrelated changes \(such as changes to build scripts\) will not trigger a rebuild. In order to incorporate build logic changes, the continuous build must be restarted manually.

### Terminating Continuous Build

If Gradle is attached to an interactive input source, such as a terminal, the continuous build can be exited by pressing`CTRL-D`\(On Microsoft Windows, it is required to also press`ENTER`or`RETURN`after`CTRL-D`\). If Gradle is not attached to an interactive input source \(e.g. is running as part of a script\), the build process must be terminated \(e.g. using the`kill`command or similar\). If the build is being executed via the Tooling API, the build can be cancelled using the Tooling API’s cancellation mechanism.

### Limitations and quirks

Continuous build is an[incubating](https://docs.gradle.org/4.6/userguide/feature_lifecycle.html)feature.

There are several issues to be aware with the current implementation of continuous build. These are likely to be addressed in future Gradle releases.

#### Build cycles

Gradle starts watching for changes just before a task executes. If a task modifies its own inputs while executing, Gradle will detect the change and trigger a new build. If every time the task executes, the inputs are modified again, the build will be triggered again. This isn’t unique to continuous build. A task that modifies its own inputs will never be considered up-to-date when run "normally" without continuous build.

If your build enters a build cycle like this, you can track down the task by looking at the list of files reported changed by Gradle. After identifying the file\(s\) that are changed during each build, you should look for a task that has that file as an input. In some cases, it may be obvious \(e.g., a Java file is compiled with`compileJava`\). In other cases, you can use`--info`logging to find the task that is out-of-date due to the identified files.

#### Restrictions with Java 9

Due to class access restrictions related to Java 9, Gradle cannot set some operating system specific options, which means that:

* On macOS, Gradle will poll for file changes every 10 seconds instead of every 2 seconds.

* On Windows, Gradle must use individual file watches \(like on Linux/Mac OS\), which may cause continuous build to no longer work on very large projects.

#### Performance and stability

The JDK file watching facility relies on inefficient file system polling on macOS \(see:[JDK-7133447](https://bugs.openjdk.java.net/browse/JDK-7133447)\). This can significantly delay notification of changes on large projects with many source files.

Additionally, the watching mechanism may deadlock under_heavy_load on macOS \(see:[JDK-8079620](https://bugs.openjdk.java.net/browse/JDK-8079620)\). This will manifest as Gradle appearing not to notice file changes. If you suspect this is occurring, exit continuous build and start again.

On Linux, OpenJDK’s implementation of the file watch service can sometimes miss file system events \(see:[JDK-8145981](https://bugs.openjdk.java.net/browse/JDK-8145981)\).

#### Changes to symbolic links

* Creating or removing symbolic link to files will initiate a build.

* Modifying the target of a symbolic link will not cause a rebuild.

* Creating or removing symbolic links to directories will not cause rebuilds.

* Creating new files in the target directory of a symbolic link will not cause a rebuild.

* Deleting the target directory will not cause a rebuild.

#### Changes to build logic are not considered

The current implementation does not recalculate the build model on subsequent builds. This means that changes to task configuration, or any other change to the build model, are effectively ignored.

