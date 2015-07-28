## Ant的属性与引用

有许多方法可以设定Ant属性,可以通过Ant任务使用属性.您可以直接在`AntBuilder`的实例设置属性。Ant的属性也可以作为一个可改变的`Map`.也可以使用Ant的任务属性,如下例所示:

**例16.13.设置Ant属性**

**build.gradle**
```gradle
ant.buildDir = buildDir
ant.properties.buildDir = buildDir
ant.properties['buildDir'] = buildDir
ant.property(name: 'buildDir', location: buildDir)
```
**build.xml**
```xml
<echo>buildDir = ${buildDir}</echo>
```

许多任务会在执行时设置属性.下面有几种方法来获取属性值,可以直接从`AntBuilder`实例获得属性,如下所示,ant的属性仍然是作为一个map:

**例16.14.获取Ant属性**

**build.xml**
```xml
<property name="antProp" value="a property defined in an Ant build"/>
```
**build.gradle**
```gradle
println ant.antProp
println ant.properties.antProp
println ant.properties['antProp']
```

设置一个ant引用的方法:

**例16.15.设置一个Ant引用**

**build.gradle**
```gradle
ant.path(id: 'classpath', location: 'libs')
ant.references.classpath = ant.path(location: 'libs')
ant.references['classpath'] = ant.path(location: 'libs')
```
**build.xml**
```xml
<path refid="classpath"/>
```

获取Ant引用的方法:

**例16.16.获取一个Ant引用**

**build.xml**
```xml
<path id="antPath" location="libs"/>
```
**build.gradle**
```gradle
println ant.references.antPath
println ant.references['antPath']
```
