# 项目报告

Gradle provides several built-in tasks which show particular details of your build. This can be useful for understanding the structure and dependencies of your build, and for debugging problems.

You can get basic help about available reporting options using`gradle help`.

### Listing projects

Running`gradle projects`gives you a list of the sub-projects of the selected project, displayed in a hierarchy.

```
❯ gradle projects
```

You also get a project report within build scans. Learn more about[creating build scans](https://guides.gradle.org/creating-build-scans/).

### Listing tasks

Running`gradle tasks`gives you a list of the main tasks of the selected project. This report shows the default tasks for the project, if any, and a description for each task.

```
❯ gradle tasks
```

By default, this report shows only those tasks which have been assigned to a task group. You can obtain more information in the task listing using the`--all`option.

```
❯ gradle tasks --all
```

### Show task usage details

Running`gradle help --task someTask`gives you detailed information about a specific task.



**Example: Obtaining detailed help for tasks**

Output of**`gradle -q help --task libs`**

```
>
 gradle -q help --task libs
Detailed task information for libs

Paths
     :api:libs
     :webapp:libs

Type
     Task (org.gradle.api.Task)

Description
     Builds the JAR

Group
     build

```

This information includes the full task path, the task type, possible command line options and the description of the given task.

### Reporting dependencies

Build scans give a full, visual report of what dependencies exist on which configurations, transitive dependencies, and dependency version selection.

```
❯ gradle myTask --scan
```

This will give you a link to a web-based report, where you can find dependency information like this.

![](https://docs.gradle.org/current/userguide/img/gradle-core-test-build-scan-dependencies.png "Build Scan dependencies report")

Learn more in[Inspecting Dependencies](https://docs.gradle.org/current/userguide/inspecting_dependencies.html).

### Listing project dependencies

Running`gradle dependencies`gives you a list of the dependencies of the selected project, broken down by configuration. For each configuration, the direct and transitive dependencies of that configuration are shown in a tree. Below is an example of this report:

```
❯ gradle dependencies
```

Concrete examples of build scripts and output available in the[Inspecting Dependencies](https://docs.gradle.org/current/userguide/inspecting_dependencies.html).

Running`gradle buildEnvironment`visualises the buildscript dependencies of the selected project, similarly to how`gradle dependencies`visualizes the dependencies of the software being built.

```
❯ gradle buildEnvironment
```

Running`gradle dependencyInsight`gives you an insight into a particular dependency \(or dependencies\) that match specified input.

```
❯ gradle dependencyInsight
```

Since a dependency report can get large, it can be useful to restrict the report to a particular configuration. This is achieved with the optional`--configuration`parameter:

### Listing project properties

Running`gradle properties`gives you a list of the properties of the selected project.



**Example: Information about properties**

Output of**`gradle -q api:properties`**

```
>
 gradle -q api:properties

------------------------------------------------------------
Project :api - The shared API for the application
------------------------------------------------------------

allprojects: [project ':api']
ant: org.gradle.api.internal.project.DefaultAntBuilder@12345
antBuilderFactory: org.gradle.api.internal.project.DefaultAntBuilderFactory@12345
artifacts: org.gradle.api.internal.artifacts.dsl.DefaultArtifactHandler_Decorated@12345
asDynamicObject: DynamicObject for project ':api'
baseClassLoaderScope: org.gradle.api.internal.initialization.DefaultClassLoaderScope@12345
buildDir: /home/user/gradle/samples/userguide/tutorial/projectReports/api/build
buildFile: /home/user/gradle/samples/userguide/tutorial/projectReports/api/build.gradle

```

### Software Model reports

You can get a hierarchical view of elements for[software model](https://docs.gradle.org/current/userguide/software_model.html)projects using the`model`task:

```
❯ gradle model
```

Learn more about[the model report](https://docs.gradle.org/current/userguide/software_model.html#model-report)in the software model documentation.

