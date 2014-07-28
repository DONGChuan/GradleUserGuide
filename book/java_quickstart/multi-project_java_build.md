# Multi-project Java build

Now let's look at a typical multi-project build. Below is the layout for the project:

Example 7.10. Multi-project build - hierarchical layout

Build layout

    multiproject/
      api/
      services/webservice/
      shared/

Note: The code for this example can be found at samples/java/multiproject which is in both the binary and source distributions of Gradle.
Here we have three projects. Project api produces a JAR file which is shipped to the client to provide them a Java client for your XML webservice. Project webservice is a webapp which returns XML. Project shared contains code used both by api and webservice.


