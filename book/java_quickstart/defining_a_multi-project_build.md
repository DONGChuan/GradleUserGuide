# 定义一个多项目构建

为了定义一个多项目构建, 你需要创建一个设置文件 ( settings file). 设置文件放在源代码的根目录, 它指定要包含哪个项目. 它的名字必须叫做 **settings.gradle**. 在这个例子中, 我们使用一个简单的分层布局. 下面是对应的设置文件:

*Example 7.11. 多项目构建 - settings.gradle file*

*settings.gradle*

    include "shared", "api", "services:webservice", "services:shared"

在第56章. 多项目构建, 你可以找到更多关于设置文件的信息.


