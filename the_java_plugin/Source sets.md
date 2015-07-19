# 22.2.资源集
Java 插件引入了__Source Set__的概念，sourceSet就是一组编译和执行的文件。 这些源文件可能包含 Java 的源文件以及一些资源文件。 其他的插件可能还会在sourceSet中包含Groovy 和Scala的源文件,sourceSet有一个与之关联的编译classpath和运行classpath。

使用sourceSet的目的是为了将源文件根据它们的目的去归档到不同的逻辑组，举个例子，你可以使用一个sourceSet来定义一个集成测试套件 也可以使用另外的源组来定义你项目的 API 和实现类。

Java 插件定义了两个标准sourceSet，分别为`main`和`test`,`main`sourceSet中包含了生产代码并且会编译并组装成 Jar 文件， `test`sourceSet包括了测试代码并且会使用JUnit或者TestNG编译和执行。它们可以是单元测试，集成测试，验收测试或者任何对你有用的测试集。
