# 资源设置

Java 插件引入了资源设置 (Source Set) 的概念, 资源设置就是一组被编译和执行在一起的源文件.
这些源文件可能包含 Java 的源文件以及一些资源文件.
其他的插件可能还会在资源设置中包含 Groovy 和 Scala 的源文件. 资源设置有一个与之关联的关于编译的 classpath 和有关运行的 classpath.

资源设置的用法之一就是将源文件归档到描述它们目的的各个逻辑组,
举个例子,
你可以使用一个资源设置来定义一个集成测试套件
也可以使用另外的资源设来定义你项目的 API 和实现类.

Java 插件定义了两个标准资源设置, 分别称为`main`和`test`, `main`资源设置中包含了生产代码并且会编译并组装成 Jar 文件,
`test`资源设置包括了测试代码并且会使用JUnit或者TestNG编译和执行. 它们可以是单元测试, 集成测试, 验收测试或者任何对你有用的测试集.


The main source set contains your production source code, which is compiled and assembled into a JAR file. The test source set contains your test source code, which is compiled and executed using JUnit or TestNG. These can be unit tests, integration tests, acceptance tests, or any combination that is useful to you.

