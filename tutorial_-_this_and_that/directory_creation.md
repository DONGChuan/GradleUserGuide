# 创建目录

有一个很常见的情况是多任务往往需要依赖于某个目录，当然您可以在这些任务开始时创建一个目录，但是这个方法太麻烦，太笨了. 这里有个很酷的解决方法，您可以使用 <u>*dependsOn*</u>，然后在 task 之间复用它再去创建目录.

**例子 14.1.使用 mkdir 创建目录**

**build.gradle**

    def classesDir = new File('build/classes')

    task resources << {
        classesDir.mkdirs()
    }
    task compile(dependsOn: 'resources') << {
        if (classesDir.isDirectory()) {
           println 'The class directory exists. I can operate'
        }
    }

    // do something

使用 **gradle -q compile**输出结果

    >gradle -q compile
    The class directory exists. I can operate
