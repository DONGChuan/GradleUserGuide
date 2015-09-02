# 定制War

下面的例子中有一些重要的自定义选项

**例25.2.定制War插件**

**build.gradle**

```
configuration{
  moreLibs
}

respositories{
  faltDir {dirs "lib"}
  mavenCentral()
}

dependencies{
  compile module(":compile:1.0") {
        dependency ":compile-transitive-1.0@jar"
        dependency ":providedCompile-transitive:1.0@jar"
  }
  providedCompile "javax.servlet:servlet-api:2.5"
    providedCompile module(":providedCompile:1.0") {
        dependency ":providedCompile-transitive:1.0@jar"
  }
  runtime ":runtime:1.0"
  providedRuntime ":providedRuntime:1.0@jar"
  testCompile "junit:junit:4.12"
  moreLibs ":otherLib:1.0"
}

war{
  from 'src/rootContent' // 增加一个目录到归档根目录
  webInf {from 'src/additionalWebInf'} // 增加一个目录到 WEB-INF 下
  classpath fileTree('additionalLibs') // 增加一个目录到 WEB-INF/lib下.
  classpath configurations.moreLibs // 增加更多地设置到 WEB-INF/lib 下.
  webXml = file('src/someWeb.xml') // 复制xml文件到 WEB-INF/web.xml.
}
```

当然,可以用一个封闭的标签定义一个文件是否存打包到War文件中.
