# 从 Maven 迁移到 Gradle

Converting a build can be scary, but you don’t have to do it alone. You can search docs, forums, and StackOverflow from[help.gradle.org](https://gradle.org/help)or reach out to the[Gradle community on the forums](https://discuss.gradle.org/c/help-discuss)if you get stuck.

[Apache Maven](https://maven.apache.org/)is a build tool for Java and other JVM-based projects that’s in widespread use, and so people that want to use Gradle often have to migrate an existing Maven build. This guide will help with such a migration by explaining the differences and similarities between the two tools' models and providing steps that you can follow to ease the process.

Contents

* [Making a case for migration](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#making_a_case_for_migration)
* [Understanding the functional differences](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#understanding_the_functional_differences)
* [Automated conversion](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#automated_conversion)
* [Bills of Materials \(BOMs\)](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#bills_of_materials_boms)
* [Provided scope and optional dependencies](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#provided_scope_and_optional_dependencies)
* [Maven profiles and properties](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#maven_profiles_and_properties)
* [Resource filtering](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#resource_filtering)
* [Integration tests](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#integration_tests)
* [Common plugins](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#common_plugins)
* [Plugins you don’t need](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#plugins_you_don_t_need)
* [Uncommon and custom plugins](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#uncommon_and_custom_plugins)
* [Next steps](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#next_steps)
* [Help improve this guide](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#help_improve_this_guide)

## [Making a case for migration](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#making_a_case_for_migration) {#making_a_case_for_migration}

The primary differences between Gradle and Maven are flexibility, performance, user experience, and dependency management. A visual overview of these aspects is available in the[Maven vs Gradle feature comparison](https://gradle.org/maven-vs-gradle).

Since Gradle 3.0, Gradle has invested heavily in making Gradle builds much faster, with features such as[build caching](https://blog.gradle.org/introducing-gradle-build-cache),[compile avoidance](https://blog.gradle.org/incremental-compiler-avoidance), and an improved incremental Java compiler. Gradle is now 2-10x faster than Maven for the vast majority of projects, even without using build caching. In-depth performance comparison and business cases for switching from Maven to Gradle can be found[here](https://gradle.org/gradle-vs-maven-performance/).

## [Understanding the functional differences](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#understanding_the_functional_differences) {#understanding_the_functional_differences}

Gradle and Maven have fundamentally different views on how to build a project. Gradle is based on a_graph of task dependencies_, where the tasks do the work. Maven uses a model of fixed, linear phases to which you can attach goals \(the things that do the work\). Despite this, migrations can be surprisingly easy because Gradle follows many of the same conventions as Maven and dependency management works in a similar way.

One especially useful feature for understanding your new Gradle build is[build scans](https://scans.gradle.com/): these are web-based snapshots of a given build that allows collaborative debugging and fine-grained performance analysis. For example, here’s the[build timeline for the Groovy build](https://scans.gradle.com/s/u7uyv3vuyhrig/timeline):

![](https://guides.gradle.org/migrating-from-maven/images/groovy-build-scan.png "groovy build scan")

You can use build scans to debug dependency resolution, Gradle task execution, and many other aspects of your build.

Once you’ve decided to go ahead with the migration, what should you do first? The best starting point is\_recording the inputs and outputs\_of your Maven build, so that you can verify that your new Gradle build is functionally equivalent.

## [Automated conversion](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#automated_conversion) {#automated_conversion}

Not only will the Gradle`init`task allow you to create a new skeleton project, but it will also automatically convert an existing Maven one to Gradle. All you have to do is run the command

```
$ gradle init
```

from the root project directory and let Gradle do its thing. That basically consists of parsing the existing POMs and generating corresponding Gradle build files plus a`settings.gradle`file if it’s a multi-project build.

You’ll find that the new Gradle build includes any custom repositories specified in the POM, your external and inter-project dependencies, the appropriate plugins \(any of`maven`,`java`, and`war`\), and more. See the user guide for[a complete list of the automatic conversion features](https://docs.gradle.org/4.6/userguide/build_init_plugin.html#sec:pom_maven_conversion_).

One thing to bear in mind is that assemblies are not automatically converted. They aren’t necessarily problematic to convert, but you will need to do some manual work.

If you’re lucky and don’t have many plugins or much in the way of customisation in your Maven build, you can simply run

```
$ gradle build
```

once the migration has completed. This will run the tests and produce the required artifacts without any extra intervention on your part.

## [Bills of Materials \(BOMs\)](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#bills_of_materials_boms) {#bills_of_materials_boms}

Maven enables sharing constraints on dependencies by defining dependencies inside dependencyManagement section in a POM file with`<packaging>pom</packaging>`. This special type of POM \(a BOM\) can then be imported into other POMs so that you have consistent library versions across your projects.

Gradle 4.6 and above supports importing BOMs, though in versions before Gradle 5.0 you must enable this explicitly by adding the following to your`settings.gradle`file:

settings.gradle

```
enableFeaturePreview("IMPROVED_POM_SUPPORT")
```

The BOM support in Gradle works similar to using`<scope>import</scope>`when depending on a BOM in Maven. In Gradle however, it is done via a regular dependency declaration on the BOM.

build.gradle

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-dependencies:1.5.8.RELEASE'
    implementation 'com.google.code.gson:gson'
    implementation 'dom4j:dom4j'
}
```

|  | Spring Boot Dependencies BOM |
| :--- | :--- |
|  | Dependencies without versions can now be resolved |

More information is available in the user manual section on[importing version recommendations from a Maven BOM](https://docs.gradle.org/4.6/userguide/managing_transitive_dependencies.html#sec:bom_import).

|  | Gradle versions 4.5 and lower do not support BOMs directly, but you can make use of them in your Gradle build files by using the[nebula-dependency-recommender-plugin](https://github.com/nebula-plugins/nebula-dependency-recommender-plugin#1-usage)from Netflix. |
| :--- | :--- |


## [Provided scope and optional dependencies](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#provided_scope_and_optional_dependencies) {#provided_scope_and_optional_dependencies}

Gradle has a lot of flexibility in the way that dependencies are managed, particularly through configurations. However, it doesn’t have a scope named`provided`out of the box unless you use the`war`plugin. The same basic behavior can be had from the[compileOnly](https://blog.gradle.org/introducing-compile-only-dependencies)configuration added in Gradle 2.12.

Regarding`optional`dependencies, Gradle 4.6+ creates dependency constraints when consuming POM files. For versions before Gradle 5.0, you must add`enableFeaturePreview('IMPROVED_POM_SUPPORT')`to_settings.gradle_.

Learn more about dependency constraints in the[Gradle user manual on managing transitive dependencies](https://docs.gradle.org/4.6/userguide/managing_transitive_dependencies.html#sec:dependency_constraints).

|  | Gradle 4.5 and lower do not support optional dependencies, but you can make use of them in your Gradle builds by using the[optional-base plugin](https://plugins.gradle.org/plugin/nebula.optional-base)from Netflix. |
| :--- | :--- |


## [Maven profiles and properties](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#maven_profiles_and_properties) {#maven_profiles_and_properties}

Maven allows you parameterize builds using properties of various sorts. Some are read-only properties of the project model, others are user-defined in the POM. It even allows you to treat system properties as project properties.

Gradle has a similar system of project properties, although it differentiates between those and system properties. You can, for example, define properties in:

* the build file

* a`gradle.properties`file in the root project directory

* a`gradle.properties`file in the`$HOME/.gradle`directory

Those aren’t the only options, so if you are interested in finding out more about how and where you can define properties,[check out the user guide](https://docs.gradle.org/4.6/userguide/build_environment.html).

One important piece of behavior you need to be aware of is what happens when the same property is defined in both the build file and one of the external properties files: the build file value takes precedence. Always. Fortunately, you can mimic the concept of profiles to provide overridable default values.

Which brings us on to Maven profiles. These are a way to enable and disable different configurations based on environment, target platform, or any other similar factor. Logically, they are nothing more than limited ‘if' statements. And since Gradle has much more powerful ways to declare conditions, it does not need to have formal support for profiles \(except in the POMs of dependencies\). You can easily get the same behavior by combining conditions with secondary build files, as you’ll see next.

Let’s say you have different deployment settings depending on environment: local development \(the default\), a test environment, and production. To add profile-like behavior, first create build files for each environment in the project root:`profile-default.gradle`,`profile-test.gradle`, and`profile-prod.gradle`. Next, add a condition similar to the following to the main build file:

```
if
 (!hasProperty('buildProfile'
)) ext.buildProfile = 'default'

apply from: "profile-${buildProfile}.gradle"
```

All you have to do then is put the environment-specific configuration, such as project properties, dependencies, etc., in the corresponding build file. To activate a particular profile, you can just pass in the relevant project property on the command line:

```
$ gradle -PbuildProfile=test build
```

Or you can set the project property another way. It’s up to you. And those conditions don’t just have to check project properties. You could check environment variables, the JDK version, the OS the build is running on, and anything else you can imagine.

One thing to bear in mind is that high level ‘if' statements make builds harder to understand and maintain, similar to the way they complicate Object-Oriented code. The same applies to profiles. Gradle offers you many better ways to avoid the extensive use of profiles that Maven often requires, for example by offering variants.

For a lengthier discussion on working with Maven profiles in Gradle, look no further than[this article](http://gradle.org/feature-spotlight-gradles-support-maven-pom-profiles)by Benjamin Muschko.

## [Resource filtering](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#resource_filtering) {#resource_filtering}

Maven has a phase called`process-resources`that has the goal`resources:resources`bound to it by default. This gives the build author an opportunity to perform variable substitution on various files, such as web resources, packaged properties files, etc.

The Java plugin for Gradle provides a`processResources`task to do the same thing. Here’s an example configuration:

build.gradle

```
processResources {
    expand(
version
: version, 
buildNumber
: currentBuildNumber)
}
```

So the left hand side of each colon is the token name and the right hand side is a project property. This variable substitution will apply to all your resource files \(the ones under`src/main/resources`usually\).

Gradle has other powerful ways for property processing. You can hook in your own filter via a closure that allows you to process the content line by line, or you can add your own`FilterReader`implementation. For more details, see the documentation for the[ContentFilterable](https://docs.gradle.org/4.6/javadoc/org/gradle/api/file/ContentFilterable.html)interface which all copy and archive tasks implement.

## [Integration tests](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#integration_tests) {#integration_tests}

Although unit tests are very useful, they can’t ensure that an application or library works as a whole. It’s easy for bugs to appear in the interactions between objects and their interactions with the environment. That’s why many projects incorporate some form of higher level testing, sometimes termed integration, functional or acceptance testing.

Maven supports these types of test by providing an extra set of phases:`pre-integration-test`,`integration-test`,`post-integration-test`, and`verify`. It also uses the Failsafe plugin rather than Surefire so that failed integration tests don’t automatically fail the build \(because you may need to clean up resources, such as a running application server\).

Another factor to consider is where you keep your integration test classes. The default approach is to mix them with your unit test classes, but this is less than ideal. A common alternative is to use profiles so that you can keep the two types of test separate.

So how should you approach migrating such a setup to Gradle? Forget plugins: source sets are your friends in this situation. A standard Java project already has two source sets for your main classes and your unit tests. Why not add an extra one for integration tests? Or even more than one for different types of integration test? Say low-level tests against a live database and higher level tests with something like FitNesse.

By declaring a new source set, Gradle automatically sets you up with corresponding configurations \(`[sourceSet]Compile`and`[sourceSet]Runtime`\) as well as compilation tasks \(`compile[SourceSet][Lang]`\) and a resource processing task \(`process[SourceSet]Resources`\). All you need to do is add a task to run the tests and ensure that the classpaths are all set up. You might also want to add tasks for starting/stopping a database or application server if your tests require something like that.

Let’s now take a look at an example so you can see what’s involved in practice:

build.gradle

```
sourceSets {
    integTest {
        compileClasspath += main.output + test.output
        runtimeClasspath += main.output + test.output
    }
}
configurations {
    integTestCompile.extendsFrom testCompile
    integTestRuntime.extendsFrom testRuntime
}
```

In the above example, I create a new source set called`integTest`. I also make sure that the application or library classes, as well as their dependencies, are included on the classpath when compiling the integration tests.

Your integration tests will probably use some third party libraries of their own, so you’ll want to add those the compilation classpath too. That’s done in the normal way in the`dependencies`block:

build.gradle

```
dependencies {

// ...

    integTestCompile 'org.codehaus.groovy:groovy-all:2.4.3'
    integTestCompile 'org.spockframework:spock-core:0.7-groovy-2.0'
, {
        exclude module: 'groovy-all'
    }
}
```

The integration tests will now compile, but there is currently no way to run them. That’s where the custom`Test`task comes in:

build.gradle

```
task integTest(
type
: Test) {
    dependsOn startApp
    finalizedBy stopApp
    testClassesDirs = files(sourceSets.integTest.output.classesDirs)
    classpath = sourceSets.integTest.runtimeClasspath
    mustRunAfter test
}
```

In the above example, I’m assuming that the integration tests run against an application server that needs to be started and shut down at the appropriate times. You can learn more about how and what to configure on the`Test`task in Gradle’s[DSL Reference](https://docs.gradle.org/4.6/dsl/org.gradle.api.tasks.testing.Test.html).

All that’s left to do at this point is incorporate the`integTest`task into your task graph, for example by having`build`depend on it. It’s really up to you how you fit it into the build. If you want to support other test types, just rinse and repeat.

## [Common plugins](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#common_plugins) {#common_plugins}

Maven and Gradle share a common approach of extending the build through plugins. Although the plugin systems are very different beneath the surface, they share many feature-based plugins, such as:

* Shade/Shadow

* Jetty

* Checkstyle

* JaCoCo

* AntRun \(see further down\)

Why does this matter? Because many plugins rely on standard Java conventions, so migration is just a matter of replicating the configuration of the Maven plugin in Gradle. As an example, here’s a simple Maven Checkstyle plugin configuration:

```
...

<
plugin
>
<
groupId
>
org.apache.maven.plugins
<
/groupId
>
<
artifactId
>
maven-checkstyle-plugin
<
/artifactId
>
<
version
>
2.17
<
/version
>
<
executions
>
<
execution
>
<
id
>
validate
<
/id
>
<
phase
>
validate
<
/phase
>
<
configuration
>
<
configLocation
>
checkstyle.xml
<
/configLocation
>
<
encoding
>
UTF-8
<
/encoding
>
<
consoleOutput
>
true
<
/consoleOutput
>
<
failsOnError
>
true
<
/failsOnError
>
<
linkXRef
>
false
<
/linkXRef
>
<
/configuration
>
<
goals
>
<
goal
>
check
<
/goal
>
<
/goals
>
<
/execution
>
<
/executions
>
<
/plugin
>

...
```

Everything outside of the configuration block can safely be ignored when migrating to Gradle. In this case, the corresponding Gradle configuration looks like the following:

```
checkstyle {
    config = resources.text.fromFile(
'
checkstyle.xml
'
, 
'
UTF-8
'
)
    showViolations = 
true

    ignoreFailures = 
false

}
```

The Checkstyle tasks are automatically added as dependencies of the`check`task, which also includes`test`. If you want to ensure that Checkstyle runs before the tests, then just specify an ordering with the mustRunAfter\(\) method:

```
test.mustRunAfter checkstyleMain, checkstyleTest
```

As you can see, the Gradle configuration is often much shorter than the Maven equivalent. You also have a much more flexible execution model since we are not longer constrained by Maven’s fixed phases.

While migrating a project from Maven, don’t forget about source sets. These often provide a more elegant solution for handling integration tests or generated sources than Maven can provide, so you should factor them into your migration plans.

### [Ant goals](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#ant_goals) {#ant_goals}

Many Maven builds rely on the AntRun plugin to customize the build without the overhead of implementing a custom Maven plugin. Gradle has no equivalent plugin because Ant is a first-class citizen in Gradle builds, via the`ant`object. For example, you can use Ant’s Echo task like this:

```
task sayHello {
    doLast {
        ant.echo 
message
: 
'
Hello!
'

    }
}
```

Even Ant properties and filesets are supported natively. To learn more, check out the[Ant chapter](https://docs.gradle.org/4.6/userguide/ant.html)of the user guide.

## [Plugins you don’t need](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#plugins_you_don_t_need) {#plugins_you_don_t_need}

It’s worth remembering that Gradle builds are typically easier to extend and customize than Maven. In this context, that means you may not need a Gradle plugin to replace a Maven one. For example, the Maven Enforcer plugin allows you to control dependency versions and environmental factors, but these things can easily be configured in a normal Gradle build script.

## [Uncommon and custom plugins](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#uncommon_and_custom_plugins) {#uncommon_and_custom_plugins}

You may come across Maven plugins that have no counterpart in Gradle, particularly if you or someone in your organisation has written a custom plugin. Such cases rely on you understanding how Gradle \(and potentially Maven\) works, because you will usually have to write your own plugin.

For the purposes of migration, there are two key types of Maven plugin:

* Those that use the Maven project object.

* Those that don’t.

Why is this important? Because if you use one of the latter, you can trivially reimplement it as a Gradle task. Simply define task inputs and outputs to correspond to the mojo parameters and convert the execution logic into a task action.

If a plugin depends on the Maven project, then you will have to rewrite it. Don’t start by considering how the Maven plugin works, but look at what problem it is trying to solve. Then try to work out how to solve that problem in Gradle. You’ll probably find that the two build models are different enough that "transcribing" Maven plugin code into a Gradle plugin just won’t be effective. On the plus side, the plugin is likely to be much easier to write than the original Maven one because Gradle has a much richer build model.

If you do need to implement custom logic, either via build files or plugins, then be sure to familiarize yourself with Gradle’s[DSL Reference](https://docs.gradle.org/4.6/dsl/), which provides comprehensive documentation on the API that you’ll be working with. It details the standard configuration blocks \(and the objects that back them\), the core types in the system \(`Project`,`Task`, etc.\), and the standard set of tasks. The main entry point is the`Project`interface as that’s the top-level object that backs the build files.

## [Next steps](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#next_steps) {#next_steps}

You did it!**Welcome to the Gradle community!**🎉

Once you have your Gradle build working functionally, it is always worth a 2nd pass to ensure that your build is fast and maintainable. See Gradle documentation on[organizing build logic](https://docs.gradle.org/4.6/userguide/organizing_build_logic.html)and[optimizing build performance](https://guides.gradle.org/performance/). It would also be good to document any decisions made for your build, such as how you store and maintain dependency versions.

If you[email us a build scan of your new Gradle build](mailto:developer-experience@gradle.com), we’ll send you a claim code for some free Gradle swag.

Finally, don’t be afraid to ask for a build scan if another build user encounters trouble with the Gradle build.

## [Help improve this guide](https://guides.gradle.org/migrating-from-maven/?_ga=2.79140304.599696663.1521685504-557532416.1521019880#help_improve_this_guide) {#help_improve_this_guide}

Have feedback or a question? Found a typo? Like all Gradle guides, help is just a GitHub issue away. Please add an issue or pull request to[gradle-guides/migrating-from-maven](https://github.com/gradle-guides/migrating-from-maven/)and we’ll get back to you.

