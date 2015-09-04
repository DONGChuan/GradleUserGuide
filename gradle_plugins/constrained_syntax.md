# 约束语法

新插件`{}`块不支持任意Groovy代码.被限制的原因是为幂等(每次产生相同的结果)和无副作用(为了Gradle随时执行的安全).


形式是:

```Gradle
plugins{
    id «plugin id»  version «plugin version»
}
```

`«plugin id»`和`«plugin version»`必须是常量,字面量,字符串.其他语句都是不允许的;他们的存在会导致编译错误.

插件`{}`块也必须在构建脚本的顶部声明.它不能被嵌套在另一个结构(例如,if语句或for循环).
