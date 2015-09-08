### 22.7.3.一些 source set 的例子

加入含有类文件的 sorce set 的 JAR：

**例22.8.为 source set 组装 JAR**

**build.gradle**

```
task intTestJar(type: Jar) {
    from sourceSets.intTest.output
}
```

为 source set 生成 javadoc:

**例22.9.为source set生成javadoc**

**build.gradle**

```
task intTestJavadoc(type: Javadoc) {
    source sourceSets.intTest.allJava
}
```

为 source set 添加一个测试套件运行测试：

**例22.8.source set 运行测试**

**build.gradle**

```gradle
task intTest(type: Test) {
    testClassesDir = sourceSets.intTest.output.classesDir
    classpath = sourceSets.intTest.runtimeClasspath
}
```
