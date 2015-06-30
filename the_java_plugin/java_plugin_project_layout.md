# 项目布局

Java 插件的默认布局如下图所示, 无论这些文件夹中有没有内容, Java 插件都会编译里面的内容, 并处理任何缺失的内容.

**表22.4.java 插件-默认布局**

目录 | 含义
--— | ---
src/main/java |  主要 Java 源码
src/main/resources | 主要资源
src/test/java | 测试 Java 源码
src/test/resources | 测试资源
src/sourceSet/java | 指定资源设置的 Java 源码
src/sourceSet/resources | 指定资源设置的资源

### 1. 改变项目布局

可以通过配置适当的资源设置来配置项目布局, 更多细节会在后面的章节中讨论, 下面是一个配置了 java 和 resource 的简单的例子


**例22.2.自定义 Java 源码布局**

**build.gradle**

```
sourceSet{
  main{
    java{
      srcDir 'src/java'
    }
    resources{
      srcDir 'src/resources'
    }
  }
}
```
