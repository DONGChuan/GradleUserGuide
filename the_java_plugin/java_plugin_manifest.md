### 22.14.1.Manifest

每个 jar 或 war 对象有一个 manifest 属性做为[Manifest](https://docs.gradle.org/2.4/javadoc/org/gradle/api/java/archives/Manifest.html)单独的实例, 
当生成存档, 一个对应MANIFEST.MF文件被写入到档案中.

**例22.15.MANIFEST.MF的定​​制**  
**build.gradle**
```
jar {
    manifest {
        attributes("Implementation-Title": "Gradle",
                   "Implementation-Version": version)
    }
}
```
你可以创建一个 manifest 的独立实例. 
您可以使用如共享 jar 之间的 manifest 的信息.

**例22.16.创建一个manifest对象**  
**build.gradle**
```
ext.sharedManifest = manifest {
    attributes("Implementation-Title": "Gradle",
               "Implementation-Version": version)
}
task fooJar(type: Jar) {
    manifest = project.manifest {
        from sharedManifest
    }
}
```
您可以合并其他 manifest 到任何 Manifest 对象. 其它清单可能是通过文件路径描述或着像上所述, 引用另一个Manifest对象.

**例22.17.独立的MANIFEST.MF一个特定的归档**  
**build.gradle**
```
task barJar(type: Jar) {
    manifest {
        attributes key1: 'value1'
        from sharedManifest, 'src/config/basemanifest.txt'
        from('src/config/javabasemanifest.txt',
             'src/config/libbasemanifest.txt') {
            eachEntry { details ->
                if (details.baseValue != details.mergeValue) {
                    details.value = baseValue
                }
                if (details.key == 'foo') {
                    details.exclude()
                }
            }
        }
    }
}
```
清单合并的顺序与声明语句的顺序相同,如果基本清单和合并的清单都为相同的密钥定义值，那么那么合并清单将会被合并,您可以通过添加在其中您可以使用一个[ManifestMergeDetails](https://docs.gradle.org/2.4/javadoc/org/gradle/api/java/archives/ManifestMergeDetails.html)实例为每个条目实体完全自定义的合并行为。声明不会立即被来自触发合并。这是延迟执行的，要么产生jar时，或要求写入effectiveManifest时.
你可以很容易地写一个清单到磁盘。
**例22.17.独立的MANIFEST.MF一个特定的存档**
**build.gradle**
```
jar.manifest.writeTo("$buildDir/mymanifest.mf")
```
