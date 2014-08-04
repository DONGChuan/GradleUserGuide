# 创建一个发行版本

(该章需加入更多内容。。。)我们同时也加入了一个发行版本, 将会送到客户端:

*Example 7.14. 多项目构建 - 发行文件*

*api/build.gradle*

    task dist(type: Zip) {
        dependsOn spiJar
        from 'src/dist'
        into('libs') {
            from spiJar.archivePath
            from configurations.runtime
        }
    }

    artifacts {
       archives dist
    }
