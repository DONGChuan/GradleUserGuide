# 构建C++可执行文件

本指南演示了如何使用Gradle的 `cpp` 插件创建一个简洁的C++可执行文件。

## [你需要什么](#what_you_ll_need) {#what_you_ll_need}

* 大约6分钟的空闲时间

* 一个文本编辑器

* 一个命令提示窗口

* 不低于1.7版本的JDK

* 一个不低于4.6版本的 [Gradle发行版本](https://gradle.org/install) 

* 一个已经安装的C++编译器。查看Gradle支持哪些 [C++工具链](https://docs.gradle.org/4.6/userguide/native_software.html#native-binaries:tool-chain-support) ，以及针对当前平台所需要下载的 [安装配置](https://docs.gradle.org/4.6/userguide/native_software.html#sec:tool_chain_installation) 。

## [布局](#layout) {#layout}

第一步，为新项目新建一个文件夹并添加 `[Gradle Wrapper](https://docs.gradle.org/4.6/userguide/gradle_wrapper.html#sec:wrapper_generation)` 。

```
$ mkdir cpp-executable
$ cd cpp-executable
$ gradle wrapper 


:wrapper

BUILD SUCCESSFUL
```

上述 `gradle wrapper` 命令行：将一个Gradle版本与一个项目进行绑定，以后就可以用 `./gradlew` 替代 `gradle` 命令行了。

新建一个最简单的 `buidl.gradle` 文件，该文件有如下内容：

`build.gradle`

```
apply plugin : 'cpp' // C++项目通过 cpp 插件启用

model {  // 所有原生构建定义都在 model 代码块内完成
    components {
        main(NativeExecutableSpec)  // 通过名称定义本地可执行文件，当前示例是 main 。决定着代码的默认路径，以及最终可执行文件名。 通过 [NativeExecutableSpec](https://docs.gradle.org/4.6/dsl/org.gradle.nativeplatform.NativeExecutableSpec.html) 指定一个可执行文件。
    }
}
```

如果运行如下命令：

```
$ ./gradlew tasks
```

你将会在输出中看到Gradle已经添加了许多任务：

```
installMainExecutable - Installs a development image of executable 'main:executable'
mainExecutable - Assembles executable 'main:executable'.

Build Dependents tasks
----------------------
assembleDependentsMain - Assemble dependents of native executable 'main'.
assembleDependentsMainExecutable - Assemble dependents of executable 'main:executable'.
buildDependentsMain - Build dependents of native executable 'main'.
buildDependentsMainExecutable - Build dependents of executable 'main:executable'.
```

注意在任务名称中使用 `Main` ，它是被 `main` 的组件直接派生的。

## [添加代码](#add_source_code) {#add_source_code}

通常，Gradle编译的C++项目将遵循更现代的布局。如果你习惯于通过不适用常规配制方法配置的构建工具来构建C++代码，那么这会对你造成麻烦。可以放心的是，Gradle在这方面是灵活配置的，如果你需要将C++项目迁移到Gradle，你可以参考用户指南的 [C++源码](https://docs.gradle.org/4.6/userguide/native_software.html#sec:cpp_sources) 部分。

在 `build.gradle` 中，你已经提前定义了可执行组件 `main` 。按照惯例，这意味着Gradle将在 `src/main/cpp` 目录中查找源文件和非导出的头文件。新建目录：

```
$ mkdir -p src/main/cpp
```

并在 `main.cpp` 放置在 `greeting.hpp` 中。

`src/main/cpp/main.cpp`

```
#include <iostream> // 标准的C++头文件可以通过Gradle使用的编译器工具链定位
#include "greeting.hpp" // 非导出头文件可以通过相对指定的C++源文件夹搜索 (示例中是 src/main/cpp)
int main(int argc, char ** argv) {
    std::cout greeting std::endl;
    return 0;
}
```

`src/main/cpp/greeting.hpp`

```
#ifndef GRADLE_GUIDE_EXAMPLE_GREETING_HPP__
#define GRADLE_GUIDE_EXAMPLE_GREETING_HPP__

namespace {
    const char * greeting = "Hello, World";
}

#endif
```

## [构建项目](#build_your_project) {#build_your_project}

为了构建项目，你可以简单的照常通过执行 `./gradlew build` ，但是如果你指明想要编译可执行文件，执行Gradle为你创建的任务：

```
$ ./gradlew mainExecutable

:compileMainExecutableMainCpp // 为了保持命令行的整洁，Gradle不在编译器输出上显示。如果你需要定位编译问题，编译器输出内容存储在 build/tmp/compileMainExecutableMainCpp/output.txt
:linkMainExecutable // 以类似的方式写入链接器的输出内容到 build/tmp/linkMainExecutable/output.txt
:mainExecutable

BUILD SUCCESSFUL
```

查看 `build` 文件夹可以看到一个 `exe` 文件夹。通常，Gradle会将所有可执行文件放置在根据组件命名的子文件夹中。在上述示例中，你会发现你的汇编可执行文件位于 `build/exe/main` 文件夹中，它会被 `main` 调用(或者在Windows下 `main.exe`)。

现在运行你新构建好的可执行文件。

```
$ ./build/exe/main/main

Hello World
```

恭喜！你已经通过Gradle编译了一个C++可执行文件。

## [总结](#summary) {#summary}

你已经创建了一个C++项目，并可以将其作为更丰富项目的基础。在这个过程中你学会了：

* 如何为C++可执行文件创建编译脚本

* 添加源码的位置

* 如何编译可执行文件，而不是编译整个项目

## [下一步](#next_steps) {#next_steps}

* 支持通过[CUnit](http://cunit.sourceforge.net/) 或者 [GoogleTest](https://github.com/google/googletest) 进行测试。Gradle将会分别为本手册中定义的可执行文件创建一个匹配 [CUnitTestSuiteSpec](https://docs.gradle.org/4.6/dsl/org.gradle.nativeplatform.test.cunit.CUnitTestSuiteSpec.html) 或 [GoogleTestTestSuiteSpec](https://docs.gradle.org/4.6/dsl/org.gradle.nativeplatform.test.googletest.GoogleTestTestSuiteSpec.html) 的组件。在用户手册中查看 [CUnit support](https://docs.gradle.org/4.6/userguide/native_software.html#native_binaries:cunit) 和 [GoogleTest support](https://docs.gradle.org/4.6/userguide/native_software.html#native_binaries:google_test) 部分查看更多细节。

* 由于没有所谓的为C++项目创建文档的“标准”方法， `cpp` 插件不提供生成文档。如果你使用流行的Doxygen工具来为C++代码添加文档，你可能需要看看 [Doxygen plugin for Gradle](https://plugins.gradle.org/plugin/org.ysb33r.doxygen)。

## [帮助改进本手册](#help_improve_this_guide) {#help_improve_this_guide}

有任何反馈或者疑问？或者发现任何错误？就像所有Gradle手册一样，帮助我们只需要Github上的一个issue。请提交一个issue或者提交PR到 [https://github.com/gradle-guides/building-cpp-executables/](https://github.com/gradle-guides/building-cpp-executables/) ，我们会尽快回复你。

