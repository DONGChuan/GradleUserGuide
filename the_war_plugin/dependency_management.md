# 依赖管理

War插件增加了名为providedCompile和providedRuntime的两个依赖配置.这两个配置有相同的作用域在编译或者运行时的配置,不同之处在于是否会将war文件归档.很重要的一点是它们都会提供配置传递.比如在任意的provided配置中添加了`commons-httpclient:commons-httpclient:3.0`,该依赖依赖于`commons-codec`,因为这个一个"provided"的配置,意味着这两个依赖都不会被加入你的WAR中,即使`commons-codec`库是一个显式的编译配置.如果不希望出现这种传递行为,`commons-httpclient:commons-httpclient:3.0@jar`这样声明provided依赖即可.
