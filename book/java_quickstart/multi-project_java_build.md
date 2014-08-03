# 多项目的 Java 构建

现在让我们看一个典型的多项目构建. 下面是项目的布局:

*Example 7.10. 多项目构建 - 分层布局*

*构建布局*

    multiproject/
      api/
      services/webservice/
      shared/


注意: 这个例子的代码可以在 samples/java/multiproject 里找到.

现在我们能有三个项目. 项目的应用程序接口 (API) 产生一个 JAR 文件,  这个文件将提供给用户, 给用户提供基于 XML 的网络服务. 项目的网络服务是一个网络应用, 它返回 XML. shared 目录包含被 api 和 webservice 共享的代码.


