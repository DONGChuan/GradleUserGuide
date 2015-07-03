## Chapter16.在Gradle中使用Ant
Gradle出色的集成了Ant.你可以使用单独的Ant任务或完整地Ant构建在你的Gradle构建.事实上,你会发现使用Gradle的构建脚本中使用Ant任务是比使用Ant的XML格式更加容易和强大的.你甚至可以将Gradle仅仅作为一个Ant的强大的脚本工具.

Ant可以分为两层.第一层是Ant语言.它给build.xml文件,处理目标,像macrodefs的特殊结构等提供语法支持.换句话说,除了Ant任务和类型的一切.Gradle了解这种语言，并允许您直接导入你的Ant的build.xml到Gradle的项目。然后，您可以Gradle任务就像他们是Ant的任务的目标.

第二层是Ant丰富的任务和类型,如javac,copy或jar.Gradle提供了集成Groovy和梦幻般的AntBuilder.

最后，由于构建脚本是Groovy脚本，你总是可以执行一个Ant构建作为一个外部进程。构建脚本可能会含有类似的语句：“ant clean compile”.execute().<sup>[[7](https://docs.gradle.org/current/userguide/ant.html#ftn.N1135F)]</sup>

您可以使用Gradle的Ant集成，作为迁移路径将构建Ant迁移到Gradle.例如,你可以通过导入现有的Ant构建开始,然后将你的依赖声明从Ant脚本移到你的build文件.最后,你可以将任务移动到你的构建文件,或者用一些gradle插件代替他们.随着时间的推移,这部分工作会慢慢完成,并且可以使用Gradle构建整个项目.

### 16.1.使用Ant任务和类型的构建
在构建脚本中，ant属性是由Gradle提供的.这是一个参考的[AntBuilder](https://docs.gradle.org/current/javadoc/org/gradle/api/AntBuilder.html)实例.AntBuilder用于从构建脚本访问Ant任务，类型和属性。
