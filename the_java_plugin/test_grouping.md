### 22.13.6.测试分组
JUnit和TestNG允许为测试方法精密分组.

对于分组JUnit的测试类与测试方法,JUnit4.8引入了类别的概念.<sup>[9](https://docs.gradle.org/current/userguide/java_plugin.html#ftn.N12A10)</sup>该测试任务允许您设定JUnit包括或者排除某些类的规范。

**例22.12.JUnit分类**
**build.gradle**
```
test {
    useJUnit {
        includeCategories 'org.gradle.junit.CategoryA'
        excludeCategories 'org.gradle.junit.CategoryB'
    }
}
```

TestNG的框架有一个非常类似的概念。在TestNG的，你可以指定不同的测试组。 <sup>[[10](https://docs.gradle.org/current/userguide/java_plugin.html#ftn.N12A26)]</sup>测试分组应包括或排除在测试任务进行配置了的测试执行.

**例22.13.TestNG分组测试**
**build.gradle**
```
test {
    useTestNG {
        excludeGroups 'integrationTests'
        includeGroups 'unitTests'
    }
}
```
