### 22.13.5.测试检测
测试任务检测哪些类是通过检查编译测试类的测试类。默认情况下它会扫描所有.calss文件.可以自定义包含/排除哪些类需不要要被扫描.所使用不同的测试框架（JUnit/ TestNG）时测试类检测使用不同的标准。
当使用JUnit，我们扫描的JUnit3和JUnit4的测试类。如果任一下列条件匹配，类被认为是一个JUnit测试类：
* 类或父类集成自TestCase或GroovyTestCase
* 类或父类有@RunWith注解
* 类或者父类中的方法有@Test注解

当使用TestNG的，我们扫描注解了@Test的方法。

需要注意的是抽象类不执行。Gradle还扫描了继承树插入测试classpath中的jar文件。

如果你不想使用测试类的检测，可以通过设置scanForTestClasses为false禁用它。这将使得测试任务只使用包含/排除找到测试类。如果scanForTestClasses是false而且额并没有包含/排除指定模式,"\*\*/\*Tests.class","\*\*/\*Test.class",与"\*\*/\*Abstract\*.class"分别为包含/排除的默认值.
