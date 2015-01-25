# 构建日志

**--profile** 参数可以收集一些构建期间的信息并保存到 build/reports/profile 目录下. 并且会以构建时间命名这些文件.

下面是一份日志.
这份日志记录了总体花费时间以及各过程花费的时间.
并以时间大小倒序排列.
并且记录了任务的执行情况.

如果采用了 buildSrc,
那么在 buildSrc/build 下同时也会生成一份日志记录记录.

![](http://gradle.org/docs/current/userguide/img/profile.png)
