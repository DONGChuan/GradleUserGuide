# 手动安装

### Step 1. [下载](https://gradle.org/releases) 最新的 Gradle

有 2 种 ZIP 文件:

* Binary-only \(bin\)

* Complete \(all\) 包含文件和源码

如果需要使用老版本的 Gradle, 请查看 [releases page](https://gradle.org/releases).

### Step 2. 解压缩

#### Linux & MacOS

在你指定的文件夹下解压 zip 文件, e.g.:

```
❯ mkdir /opt/gradle
❯ unzip -d /opt/gradle gradle-4.6-bin.zip
❯ ls /opt/gradle/gradle-4.6
LICENSE  NOTICE  bin  getting-started.html  init.d  lib  media
```

#### Windows

创建一个新的目录`C:\Gradle`. 解压 Zip 文件, 并将`gradle-4.6`文件夹直接复制到新创建的目录.

### Step 3. 配置系统环境

首先需要添加环境变量`GRADLE_HOME`. 指向解压后的文件所在的文件夹. 然后添加_`GRADLE_HOME/bin`_ 到环境变量`PATH` 中.

#### Linux & MacOS

配置`PATH`指向解压后的 `bin`文件目录, e.g.:

```
❯ export PATH=$PATH:/opt/gradle/gradle-4.6/bin
```



