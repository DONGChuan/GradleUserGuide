### 22.4.1.改变项目布局
可以通过配置适当的源组配置项目布局，更多细节会在后面的章节中讨论，下面是一个配置了 java 和 resource 的简单的例子

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
