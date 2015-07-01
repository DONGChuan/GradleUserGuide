# 使用其他的脚本配置任意对象

您也可以使用其他的构建脚本配置任意的对象.

**例子: 14.5.使用别的脚本配置配置对象**

**build.gradle**

    task config << {
       def pos = new java.text.FieldPosition(10)

       // 使用另一个脚本
       apply from: 'other.gradle', to: pos
       println pos.beginIndex
       println pos.endIndex

    }

**other.gradle**

    beginIndex = 1
    endIndex = 5


**使用 gradle -q configure 输出**

    > gradle -q configure
    1
    5


