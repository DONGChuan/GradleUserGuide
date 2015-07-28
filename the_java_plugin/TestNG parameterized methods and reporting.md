#### 22.13.7.1.TestNG的参数化方法和报告
TestNG支持[参数化方法](http://testng.org/doc/documentation-main.html#parameters),允许一个特定的测试方法使用不同的输入被执行多次。Gradle会在测试报告中包含该方法的参数值.
给出一个叫aTestMethod的测试方法,该方法有两个参数,在测试报告中会根据名字报告:aTestMethod(toStringValueOfParam1, toStringValueOfParam2). 这很容易识别的参数值的特定迭代.
