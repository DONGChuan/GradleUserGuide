# Locating files

使用 `Project.file()` 方法能够相对项目目录定位一个文件

**例 16.1. 定位文件**

**build.gradle**
```
// 使用一个相对路径

File configFile = file('src/config.xml')

// 使用一个绝对路径

configFile = file(configFile.absolutePath)

// 使用一个项目路径的文件对象 

configFile = file(new File('src/config.xml'))`
```


`file()` 方法接收任何形式的对象参数.它会将参数值转换为一个绝对文件对象,一般情况下，你可以传递一个 `String` 或者一个 `File` 实例.如果传递的路径是个绝对路径,它会被直接构造为一个文件实例.否则,会被构造为项目目录加上传递的目录的文件对象.另外,`file()函数`也能识别`URL`,例如 file:/some/path.xml.


这个方法非常有用,它将参数值转换为一个绝对路径文件.所以请尽量使用 `new File(somePath)` , 因为 `file()`  总是相对于当前项目路径计算传递的路径,然后加以矫正.因为当前工作区间目录依赖于用户以何种方式运行 Gradle.













