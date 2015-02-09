# 可有可无的圆括号
在调用方法时，圆括号可有可无，是个可选的.

**例子: 13.6.不使用圆括号调用方法**

**build.gradle**

    test.systemProperty 'some.prop', 'value'
    test.systemProperty('some.prop', 'value')


