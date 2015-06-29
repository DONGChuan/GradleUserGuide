# 配置任意对象
您可以使用下面方法配置任意的对象.

**例子 14.4.配置任意对象**

**build.gradle**

    task configure << {
       def pos = configure(new java.text.FieldPosition(10)) {
           beginIndex = 1
           endIndex = 5
       }

       println pos.beginIndex
       println pos.endIndex

    }

**使用 gradle -q configure 输出**

    > gradle -q configure
    1
    5
