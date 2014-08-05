# Dependency configurations

In Gradle dependencies are grouped into configurations. A configuration is simply a named set of dependencies. We will refer to them as dependency configurations. You can use them to declare the external dependencies of your project. As we will see later, they are also used to declare the publications of your project.

The Java plugin defines a number of standard configurations. These configurations represent the classpaths that the Java plugin uses. Some are listed below, and you can find more details in Table 23.5, “Java plugin - dependency configurations”.

compile

The dependencies required to compile the production source of the project.

runtime

The dependencies required by the production classes at runtime. By default, also includes the compile time dependencies.

testCompile

The dependencies required to compile the test source of the project. By default, also includes the compiled production classes and the compile time dependencies.

testRuntime

The dependencies required to run the tests. By default, also includes the compile, runtime and test compile dependencies.

Various plugins add further standard configurations. You can also define your own custom configurations to use in your build. Please see Section 50.3, “Dependency configurations” for the details of defining and customizing dependency configurations.

