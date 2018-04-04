Gradle provides a powerful mechanism to allow customizing the build based on the current environment. This mechanism also supports tools that wish to integrate with Gradle.

Note that this is completely different from the “`init`” task provided by the “`build-init`” incubating plugin \(see[Build Init Plugin](https://docs.gradle.org/4.6/userguide/build_init_plugin.html)\).

## Basic usage

Initialization scripts \(a.k.a._init scripts_\) are similar to other scripts in Gradle. These scripts, however, are run before the build starts. Here are several possible uses:

* Set up enterprise-wide configuration, such as where to find custom plugins.

* Set up properties based on the current environment, such as a developer’s machine vs. a continuous integration server.

* Supply personal information about the user that is required by the build, such as repository or database authentication credentials.

* Define machine specific details, such as where JDKs are installed.

* Register build listeners. External tools that wish to listen to Gradle events might find this useful.

* Register build loggers. You might wish to customize how Gradle logs the events that it generates.

One main limitation of init scripts is that they cannot access classes in the`buildSrc`project \(see[the section called “Build sources in the`buildSrc`project”](https://docs.gradle.org/4.6/userguide/organizing_build_logic.html#sec:build_sources)for details of this feature\).

## Using an init script

There are several ways to use an init script:

* Specify a file on the command line. The command line option is`-I`or`--init-script`followed by the path to the script. The command line option can appear more than once, each time adding another init script.

* Put a file called`init.gradle`in the_`USER_HOME`_`/.gradle/`directory.

* Put a file that ends with`.gradle`in the_`USER_HOME`_`/.gradle/init.d/`directory.

* Put a file that ends with`.gradle`in the_`GRADLE_HOME`_`/init.d/`directory, in the Gradle distribution. This allows you to package up a custom Gradle distribution containing some custom build logic and plugins. You can combine this with the[Gradle wrapper](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html)as a way to make custom logic available to all builds in your enterprise.

If more than one init script is found they will all be executed, in the order specified above. Scripts in a given directory are executed in alphabetical order. This allows, for example, a tool to specify an init script on the command line and the user to put one in their home directory for defining the environment and both scripts will run when Gradle is executed.

## Writing an init script

Similar to a Gradle build script, an init script is a Groovy script. Each init script has a[`Gradle`](https://docs.gradle.org/4.6/dsl/org.gradle.api.invocation.Gradle.html)instance associated with it. Any property reference and method call in the init script will delegate to this`Gradle`instance.

Each init script also implements the[`Script`](https://docs.gradle.org/4.6/dsl/org.gradle.api.Script.html)interface.

### Configuring projects from an init script

You can use an init script to configure the projects in the build. This works in a similar way to configuring projects in a multi-project build. The following sample shows how to perform extra configuration from an init script_before_the projects are evaluated. This sample uses this feature to configure an extra repository to be used only for certain environments.



**Example: Using init script to perform extra configuration before projects are evaluated**

`build.gradle`

```
repositories {
    mavenCentral()
}

task showRepos {
    doLast {
        println 
"All repos:"

        println repositories.collect { it.name }
    }
}

```

`init.gradle`

```
allprojects {
    repositories {
        mavenLocal()
    }
}

```

Output of**`gradle --init-script init.gradle -q showRepos`**

```
>
 gradle --init-script init.gradle -q showRepos
All repos:
[MavenLocal, MavenRepo]
```

## External dependencies for the init script

In[the section called “External dependencies for the build script”](https://docs.gradle.org/4.6/userguide/organizing_build_logic.html#sec:build_script_external_dependencies)it was explained how to add external dependencies to a build script. Init scripts can also declare dependencies. You do this with the`initscript()`method, passing in a closure which declares the init script classpath.



**Example: Declaring external dependencies for an init script**

`init.gradle`

```
initscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath group: 
'org.apache.commons'
, name: 
'commons-math'
, version: 
'2.0'

    }
}

```

The closure passed to the`initscript()`method configures a[`ScriptHandler`](https://docs.gradle.org/4.6/javadoc/org/gradle/api/initialization/dsl/ScriptHandler.html)instance. You declare the init script classpath by adding dependencies to the`classpath`configuration. This is the same way you declare, for example, the Java compilation classpath. You can use any of the dependency types described in[Declaring Dependencies](https://docs.gradle.org/4.6/userguide/declaring_dependencies.html), except project dependencies.

Having declared the init script classpath, you can use the classes in your init script as you would any other classes on the classpath. The following example adds to the previous example, and uses classes from the init script classpath.



**Example: An init script with external dependencies**

`init.gradle`

```
import
 org.apache.commons.math.fraction.Fraction

initscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath group: 
'org.apache.commons'
, name: 
'commons-math'
, version: 
'2.0'

    }
}

println Fraction.ONE_FIFTH.multiply(
2
)

```

Output of**`gradle --init-script init.gradle -q doNothing`**

```
>
 gradle --init-script init.gradle -q doNothing
2 / 5

```

## Init script plugins

Similar to a Gradle build script or a Gradle settings file, plugins can be applied on init scripts.



**Example: Using plugins in init scripts**

`init.gradle`

```
apply plugin:EnterpriseRepositoryPlugin


class
 EnterpriseRepositoryPlugin 
implements
 Plugin
<
Gradle
>
 {

    
private
static
 String ENTERPRISE_REPOSITORY_URL = 
"https://repo.gradle.org/gradle/repo"
void
 apply(Gradle gradle) {
        
// ONLY USE ENTERPRISE REPO FOR DEPENDENCIES

        gradle.allprojects{ project -
>

            project.repositories {

                
// Remove all repositories not pointing to the enterprise repository url

                all { ArtifactRepository repo -
>
if
 (!(repo 
instanceof
 MavenArtifactRepository) ||
                          repo.url.toString() != ENTERPRISE_REPOSITORY_URL) {
                        project.logger.lifecycle 
"Repository ${repo.url} removed. Only $ENTERPRISE_REPOSITORY_URL is allowed"

                        remove repo
                    }
                }

                
// add the enterprise repository

                maven {
                    name 
"STANDARD_ENTERPRISE_REPO"

                    url ENTERPRISE_REPOSITORY_URL
                }
            }
        }
    }
}

```

`build.gradle`

```
repositories{
    mavenCentral()
}

 task showRepositories {
     doLast {
         repositories.each {
             println 
"repository: ${it.name} ('${it.url}')"

         }
     }
}

```

Output of**`gradle -q -I init.gradle showRepositories`**

```
>
 gradle -q -I init.gradle showRepositories
repository: STANDARD_ENTERPRISE_REPO ('https://repo.gradle.org/gradle/repo')
```

The plugin in the init script ensures that only a specified repository is used when running the build.

When applying plugins within the init script, Gradle instantiates the plugin and calls the plugin instance’s[`Plugin.apply(T)`](https://docs.gradle.org/4.6/javadoc/org/gradle/api/Plugin.html#apply-T-)method. The`gradle`object is passed as a parameter, which can be used to configure all aspects of a build. Of course, the applied plugin can be resolved as an external dependency as described in[the section called “External dependencies for the init script”](https://docs.gradle.org/4.6/userguide/init_scripts.html#sec:custom_classpath)

