### Creating new Gradle builds

Use the built-in`gradle init`task to create a new Gradle builds, with new or existing projects.

```
❯ gradle init
```

Most of the time you’ll want to specify a project type. Available types include`basic`\(default\),`java-library`,`java-application`, and more. See[init plugin documentation](https://docs.gradle.org/4.6/userguide/build_init_plugin.html)for details.

```
❯ gradle init --type java-library
```

### Standardize and provision Gradle

The built-in`gradle wrapper`task generates a script,`gradlew`, that invokes a declared version of Gradle, downloading it beforehand if necessary.

```
❯ gradle wrapper --gradle-version=4.4
```

You can also specify`--distribution-type=(bin|all)`,`--gradle-distribution-url`,`--gradle-distribution-sha256-sum`in addition to`--gradle-version`. Full details on how to use these options are documented in the[Gradle wrapper section](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html).

