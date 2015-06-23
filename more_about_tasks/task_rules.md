# Task rules

Sometimes you want to have a task whose behavior depends on a large or infinite number value range of parameters. A very nice and expressive way to provide such tasks are task rules:

Example 14.25. Task rule

build.gradle
tasks.addRule("Pattern: ping<ID>") { String taskName ->
    if (taskName.startsWith("ping")) {
        task(taskName) << {
            println "Pinging: " + (taskName - 'ping')
        }
    }
}
Output of gradle -q pingServer1
> gradle -q pingServer1
Pinging: Server1
The String parameter is used as a description for the rule, which is shown with gradle tasks.

Rules are not only used when calling tasks from the command line. You can also create dependsOn relations on rule based tasks:

Example 14.26. Dependency on rule based tasks

build.gradle
tasks.addRule("Pattern: ping<ID>") { String taskName ->
    if (taskName.startsWith("ping")) {
        task(taskName) << {
            println "Pinging: " + (taskName - 'ping')
        }
    }
}

task groupPing {
    dependsOn pingServer1, pingServer2
}
Output of gradle -q groupPing
> gradle -q groupPing
Pinging: Server1
Pinging: Server2
If you run “gradle -q tasks” you won't find a task named “pingServer1” or “pingServer2”, but this script is executing logic based on the request to run those tasks.
