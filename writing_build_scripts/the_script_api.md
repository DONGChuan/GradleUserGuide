# 脚本 API
当 Gradle 执行一个脚本时，它会将这个脚本编译为实现了 [Script](http://gradle.org/docs/current/dsl/org.gradle.api.Script.html) 的类. 也就是说所有的属性和方法都是在 Script 接口中声明的，由于你的脚本实现了 Script 接口，所以你可以在自己的脚本中使用它们.
