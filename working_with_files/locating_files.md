# Locating files

You can locate a file relative to the project directory using the `Project.file()` method.

使用 `Project.file()`方法相对项目目录定位一个文件

**例子 16.1. 定位文件**

**build.gradle**
```
// Using a relative path

File configFile = file('src/config.xml')

// Using an absolute path

configFile = file(configFile.absolutePath)

// Using a File object with a relative path

configFile = file(new File('src/config.xml'))`
```


You can pass any object to the file() method, and it will attempt to convert the value to an absolute File object. Usually, you would pass it a String or File instance. If this path is an absolute path, it is used to construct a File instance. Otherwise, a File instance is constructed by prepending the project directory path to the supplied path. The file() method also understands URLs, such as file:/some/path.xml.

你`file()函数`接收任何形式的对象参数.它会将参数值转换为一个绝对文件对象,一般情况下，您可以传递一个 `String` 或者一个 `File` 实例.如果传递的路径是个绝对路径,它通常会被直接构造为一个文件实例.否则,会被构造为项目文件目录加上传递的目录的文件对象.另外,`file()函数`也能识别`URLs`,例如 file:/some/path.xml.

Using this method is a useful way to convert some user provided value into an absolute File. It is preferable to using new File(somePath), as file() always evaluates the supplied path relative to the project directory, which is fixed, rather than the current working directory, which can change depending on how the user runs Gradle.

这个方法非常有用,它将参数值转换为一个绝对路径文件.使用`new File(somePath)`是最合适的,因为 `file()`总是相对于当前项目路径计算传递的路径,然后加以矫正.因为当前工作区间目录依赖于用户以何种方式运行Gradle.













