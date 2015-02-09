# 局部变量
局部变量使用关键字 def 来声明，其只在声明它的地方可见 . 局部变量是 Groovy 语言的一个基本特性.

**例子 13.2 . 使用局部变量**

        def dest = "dest"

        task copy(type: Copy) {
              form "source"
              into dest

        }



