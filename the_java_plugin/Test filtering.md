### 22.13.3.测试过滤
从Gradle1.10开始,可以根据测试任务名进行特点的任务测试,过滤与在构建脚本的段落中引入/排除测试任务(-Dtest.single, test.include and friends)是两种不同的机制.后者是基于文件,如测试实现类的物理位置.选择文件级的测试会不支持那些被测试等级过滤掉的一些有趣的测试脚本.下面的这些有些已经被实现,有些是将来会实现的:
- 过滤特定等级的试验方法;执行单个测试方法
- 通配符"*"支持任意字符匹配
- 命令行选项"--tests"用以设置测试过滤器.对经典的'执行单一测试方法'用例尤其有用.当使用命令行选项的时候,在构建脚本中声明的包含过滤器被忽略。
- Gradle过滤测试框架API的限制.一些高级的综合性测试无法完全兼容测试过滤,但是,绝大多是测试和用例可以被熟练地处理.
- 测试过滤取代基于选择文件的测试,后者在未来可能会完全被取代.我们会完善测试过滤的API,并增加不同的过滤器

**例22.11.过滤测试的构建脚本吗** **build.gradle**

```
test{
    filter{
        //包括在测试的任何具体方法
        includeTestsMatching "*UiCheck"

        //包括所有包种的测试
        includeTestsMatching "org.gradle.internal.*"

        //包括所有的集成测试
        includeTestsMatching "*IntegTest"
    }
}
```

要了解更多详细信息和示例，请参见[TestFilter](https://docs.gradle.org/current/javadoc/org/gradle/api/tasks/testing/TestFilter.html)。 使用命令行选项的一些例子:
* gradle test --tests org.gradle.SomeTest.someSpecificFeature
* gradle test --tests \*SomeTest.someSpecificFeature
* gradle test --tests \*SomeSpecificTest
* gradle test --tests all.in.specific.package\*
* gradle test --tests \*IntegTest
* gradle test --tests \*IntegTest\*ui\*
* gradle someTestTask --tests \*UiTest someOtherTestTask --tests \*WebTest\*ui
