# Selecting which build to execute

When you run the gradle command, it looks for a build file in the current directory. You can use the -b option to select another build file. If you use -b option then settings.gradle file is not used. Example:

Example 11.5. Selecting the project using a build file

subdir/myproject.gradle

    task hello << {
        println "using build file '$buildFile.name' in '$buildFile.parentFile.name'."
    }

Output of gradle -q -b subdir/myproject.gradle hello

> gradle -q -b subdir/myproject.gradle hello
using build file 'myproject.gradle' in 'subdir'.
Alternatively, you can use the -p option to specify the project directory to use. For multi-project builds you should use -p option instead of -b option.

Example 11.6. Selecting the project using project directory

Output of gradle -q -p subdir hello
> gradle -q -p subdir hello
using build file 'build.gradle' in 'subdir'.
