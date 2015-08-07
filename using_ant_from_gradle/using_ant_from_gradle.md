## 在 Gradle中使用Ant

Gradle 出色的集成了 Ant. 你可以在 Gradle 构建时使用单独的 Ant 任务或完整的 Ant 构建. 事实上, 你会发现在 Gradle 构建脚本中使用Ant任务远比直接使用 Ant 的 XML 格式更加容易和强大. 你甚至可以将 Gradle 仅仅作为一个强大的 Ant 脚本工具.

Ant 可以分为两层. 第一层是 Ant 语言. 它给 build.xml 文件, 处理目标, 像 macrodefs 的特殊构造等提供语法支持. 换句话说, 除了 Ant 任务和类型, 其余一切都支持. Gradle 了解这种语言, 并允许用户直接导入 Ant 的 build.xml 到 Gradle 的项目下. 然后, 你可以 像 Gradle 任务一样直接使用这些 Ant 构建.

第二层是 Ant 丰富的任务和类型, 如 javac, copy 或 jar. Gradle 提供了基于 Groovy 的集成以及梦幻般的 AntBuilder.

最后，由于构建脚本是 Groovy 脚本, 你总是可以执行一个 Ant 构建作为一个外部进程. 构建脚本可能会含有类似的语句：“ant clean compile”.execute().<sup>[[7](https://docs.gradle.org/current/userguide/ant.html#ftn.N1135F)]</sup>

你可以使用 Gradle 的 Ant 集成，作为迁移路径将构建 Ant 迁移到 Gradle. 例如, 你可以从通过导入现有的 Ant 构建开始, 然后将你的依赖声明从 Ant 脚本迁移到你的 build 文件. 最后, 你可以将任务迁移到你的构建文件, 或者用一些 Gradle 插件代替他们. 随着时间的推移, 这部分工作会一步一步地完成, 并且你可以在整个进程中有一个运行良好的 Gradle 构建.
