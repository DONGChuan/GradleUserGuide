### 22.7.3.一些source set的例子
加入含有类文件的sorce set的JAR：

**例22.8.为source set组装JAR**

**build.gradle**

```
task intTestJar(type: Jar) {
    from sourceSets.intTest.output
}
```

为source set生成javadoc:

**例22.9.为source set生成javadoc**

**build.gradle**

```
task intTestJavadoc(type: Javadoc) {
    source sourceSets.intTest.allJava
}
```

为source set添加一个测试套件运行测试：

**例22.8.source set运行测试**

**build.gradle**

```gradle
task intTest(type: Test) {
    testClassesDir = sourceSets.intTest.output.classesDir
    classpath = sourceSets.intTest.runtimeClasspath
}
```
