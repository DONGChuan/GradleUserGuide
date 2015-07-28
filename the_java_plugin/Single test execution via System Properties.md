### 22.13.4.通过系统属性执行单独测试
> 如上所述该机制已经被测试过滤取代

> **译者注:被取代的东西就先不翻译了**

Setting a system property of taskName.single = testNamePattern will only execute tests that match the specified testNamePattern. The taskName can be a full multi-project path like “:sub1:sub2:test” or just the task name. The testNamePattern will be used to form an include pattern of “\*\*/testNamePattern\*.class”;. If no tests with this pattern can be found an exception is thrown. This is to shield you from false security. If tests of more than one subproject are executed, the pattern is applied to each subproject. An exception is thrown if no tests can be found for a particular subproject. In such a case you can use the path notation of the pattern, so that the pattern is applied only to the test task of a specific subproject. Alternatively you can specify the fully qualified task name to be executed. You can also specify multiple patterns. Examples:

* gradle -Dtest.single=ThisUniquelyNamedTest test
* gradle -Dtest.single=a/b/ test
* gradle -DintegTest.single=\*IntegrationTest integTest
* gradle -Dtest.single=:proj1:test:Customer build
* gradle -DintegTest.single=c/d/ :proj1:integTest
