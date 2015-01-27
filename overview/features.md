# 特点

这里简述下 Gradle 的特点.

**1. 声明式构建和合约构建**

Gradle 的核心是基于 Groovy 的
领域特定语言 (DSL), 具有十分优秀的扩展性. Gradle 通过提供可以随意集成的声明式语言元素将声明性构建推到了一个新的高度. 这些元素还提供了对 Java, Groovy, OSGi, 网络和Scala 等项目的支持. 而且, 基于这种声明式语言的可扩展性.  你可以添加自己的语言元素或加强现有的语言元素, 从而提供简洁, 易于维护和易于理解的构建.


**2. 基于依赖的编程语言**

声明式语言位于通用任务图 ( general purpose task graph ) 的顶端，它可以被充分利用在你的构建中. 它具有强大的灵活性, 可以满足使用者对 Gradle 的一些特别的需求.


**3. 让构建结构化**

Gradle 的易适应性和丰富性可让你在构建里直接套用通用的设计原则. 例如, 你可以非常容易容易的使用一些可重用的组件来构成你的构建. Inline stuff where unnecessary indirections would be inappropriate. 不要强行分离已经结合在一起的部分 (例如, 在你的项目层次结构中). 避免使构建难以维护. 总之, 你可以创建一个结构良好，易于维护, 易于理解的构建.

**4. API深化**

你会非常乐意在整个构建执行的生命周期中使用 Gradle, 因为Gradle 允许你管理和定制它的配置和执行行为.

**5. Gradle scales**

Gradle scales very well.  不管是简单的独立项目还是大型的多项目构建, 它都能显著的提高效率. 它不仅可以提供最先进的构建功能，还可以解决许多大公司碰到的构建 性能低下的问题.


**6. 多项目构建**

Gradle 对多项目的支持是非常出色的. 它允许你模拟在多项目构建中项目的关系，这正是你所要关注的地方. Gradle 遵从你的布局而是去违反它.

Gradle 提供了局部构建的功能. 如果你构建一个单独的子项目, Gradle 会构建这个子项目依赖的所有子项目. 你也可以选择依赖于另一个特别的子项目重新构建这些子项目. 这样在一些大型项目里就可以节省非常多的时间.

**7. 多种方式来管理你的依赖**

不同的团队有不同的管理外部依赖的方法. Gradle 对于任何管理策略都提供了合适的支持. 从远程 Maven 和 Ivy 库的依赖管理到本地文件系统的 jars 或者 dirs.

**8. Gradle 是第一个构建整合工具**

Ant tasks are first class citizens. Even more interesting, Ant projects are first class citizens as well. Gradle provides a deep import for any Ant project, turning Ant targets into native Gradle tasks at runtime. You can depend on them from Gradle, you can enhance them from Gradle, you can even declare dependencies on Gradle tasks in your build.xml. The same integration is provided for properties, paths, etc ...

Gradle fully supports your existing Maven or Ivy repository infrastructure for publishing and retrieving dependencies. Gradle also provides a converter for turning a Maven pom.xml into a Gradle script. Runtime imports of Maven projects will come soon.

**9. 易于迁移**

Gradle 可以兼容任何结构. Therefore you can always develop your Gradle build in the same branch where your production build lives and both can evolve in parallel. We usually recommend to write tests that make sure that the produced artifacts are similar. That way migration is as less disruptive and as reliable as possible. This is following the best-practices for refactoring by applying baby steps.

**10. Groovy**

Gradle's build scripts are written in Groovy, not XML. But unlike other approaches this is not for simply exposing the raw scripting power of a dynamic language. That would just lead to a very difficult to maintain build. The whole design of Gradle is oriented towards being used as a language, not as a rigid framework. And Groovy is our glue that allows you to tell your individual story with the abstractions Gradle (or you) provide. Gradle provides some standard stories but they are not privileged in any form. This is for us a major distinguishing features compared to other declarative build systems. Our Groovy support is also not just some simple coating sugar layer. The whole Gradle API is fully groovynized. Only by that using Groovy is the fun and productivity gain it can be.

**10. The Gradle wrapper**

The Gradle Wrapper allows you to execute Gradle builds on machines where Gradle is not installed. This is useful for example for some continuous integration servers. It is also useful for an open source project to keep the barrier low for building it. The wrapper is also very interesting for the enterprise. It is a zero administration approach for the client machines. It also enforces the usage of a particular Gradle version thus minimizing support issues.

**11. 免费和开源**

Gradle 是一个开源项目, 遵循 ASL 许可.

