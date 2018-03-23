# 使用插件

Gradle includes a range of plugins, and many, many more are available at[the Gradle plugin portal](http://plugins.gradle.org/). One of the plugins included with the distribution is the`base`plugin. Combined with a core type called[`Zip`](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.bundling.Zip.html), you can create a zip archive of your project with a configured name and location.

Add the`base`plugin to your`build.gradle`file using the`plugins`syntax. Be sure to add the`plugins {}`block at the top of the file.

```
plugins {
    id 
"
base
"

}

... rest of the build file ...
```

Now add a task that creates a zip archive from the`src`directory.

```
task zip(
type
: Zip, 
group
: 
"
Archive
"
, 
description
: 
"
Archives sources in a zip file
"
) {
    from 
"
src
"

}
```

The`base`plugin works with the settings to create an archive file called`basic-demo-1.0.zip`in the`build/distributions`folder.

In this case, simply run the new`zip`task and see that the generated zip file is where you expect.

```
❯ ./gradlew zip
:zip

BUILD SUCCESSFUL in 0s
1 actionable task: 1 executed
```



