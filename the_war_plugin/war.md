# War

War任务默认会把`src/main/webapp`的内容复制到归档目录的根目录.webapp文件夹下会包含一个`WEB-INF`子文件夹,里面可能会有一个web.xml文件.编译后的class文件会在`WEB-INF/classes`下,所有runtime<sup>[[13](https://docs.gradle.org/current/userguide/war_plugin.html#ftn.N1325D)]</sup>的依赖配置会被拷贝至`WEB-INF/lib`下.

API文档中有更多关于[War](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.War.html)的信息.

---
[[13](https://docs.gradle.org/current/userguide/war_plugin.html#N1325D)]`runtime`配置扩展了`compile`配置.
