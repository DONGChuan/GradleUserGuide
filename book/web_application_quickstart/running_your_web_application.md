# Running your web application

To run your web application, you apply the Jetty plugin to your project:

Example 10.2. Running web application with Jetty plugin

build.gradle

    apply plugin: 'jetty'

This also applies the War plugin to your project. Running gradle jettyRun will run your web application in an embedded Jetty web container. Running gradle jettyRunWar will build the WAR file, and then run it in an embedded web container.

TODO: which url, configure port, uses source files in place and can edit your files and reload.

