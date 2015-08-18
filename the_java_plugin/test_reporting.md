### 22.13.7.测试报告
测试任务默认生成以下结果.
+ 一份HTML测试报告
+ 一个与Ant的JUnit测试报告任务兼容的XML.这个格式与许多其他服务兼容,如CI serves
+ 结果是有效的二进制,测试任务会从这些二进制结果生成其他结果。
有一个独立的[TestReport](https://docs.gradle.org/2.4/dsl/org.gradle.api.tasks.testing.TestReport.html)任务类型会根据一些Test任务实例生成的二进制源码生成一个HTML报告.使用这种测试类型,需要定义一个destinationDir,里面包括测试结果的报告.下面是一个示例，它产生一个从子项目的单元测试组合而成的报告：
**例22.14.创建单元测试报告子项目**
**build.gradle**
```
subprojects {
    apply plugin: 'java'

    // Disable the test report for the individual test task
    test {
        reports.html.enabled = false
    }
}

task testReport(type: TestReport) {
    destinationDir = file("$buildDir/reports/allTests")
    // Include the results from the `test` task in all subprojects
    reportOn subprojects*.test
}
```
应该注意的是，TestReport型组合来自多个测试任务的结果，需要聚集个别测试类的结果。这意味着，如果一个给定的测试类是由多个测试任务执行时，测试报告将会包括那些类,但是很难区分该输出结果分别是出自哪个类.
