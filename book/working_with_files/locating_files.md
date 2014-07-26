# Locating files

You can locate a file relative to the project directory using the `Project.file()` method.

Example 16.1. Locating files

build.gradle

// Using a relative path

File configFile = file('src/config.xml')

// Using an absolute path

configFile = file(configFile.absolutePath)

// Using a File object with a relative path

configFile = file(new File('src/config.xml'))`

You can pass any object to the file() method, and it will attempt to convert the value to an absolute File object. Usually, you would pass it a String or File instance. If this path is an absolute path, it is used to construct a File instance. Otherwise, a File instance is constructed by prepending the project directory path to the supplied path. The file() method also understands URLs, such as file:/some/path.xml.

Using this method is a useful way to convert some user provided value into an absolute File. It is preferable to using new File(somePath), as file() always evaluates the supplied path relative to the project directory, which is fixed, rather than the current working directory, which can change depending on how the user runs Gradle.

