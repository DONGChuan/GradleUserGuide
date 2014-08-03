# 特点

(待翻译)
这里简述下 Gradle 的特点.

**1. 声明性构建和合约构建**

Gradle 的核心是它有丰富的扩展的基于 Groovy 的
领域特定语言 (DSL). Gradle 通过提供可以随意集成的声明性语言元素将声明性构建推到了一个新的高度. 这些元素还提供了对 Java, Groovy, OSGi,  网络和Scala项目的支持. 而且, 这种声明性语言是可扩展的.  添加您自己的新的语言元素或加强现有的. 从而提供简洁, 易于维护和易于理解的构建.


**2. 基于依赖的编程语言**

声明性语言依赖于一个多用途的任务图表，它可以被充分利用在你的构建上中. 它具有强大的灵活性, 以适应你对 Gradle 的独特需求.


**3. 让您的构建结构更好**

Gradle 的易适应性和丰富性可让你套用通用的设计原则. 例如, 很容易通过可重要的组件来构成你的构建. Inline stuff where unnecessary indirections would be inappropriate.  不要强行分开已经结合在一起的部分 (例如, 在你的项目层次结构中). 从而避免了使你的构建难以维护. 最后, 你可以创建一个结构良好，易于维护, 易于理解的构建.

**4. API深化**

你会非常乐意在整个构建执行的生命周期中使用 Gradle, 因为Gradle 允许你管理和定制它的配置和执行行为.

Gradle scales

Gradle scales very well. 它显著了提高了生产率, 从简单的独立项目到大型的多项目构建. 可以提供最先进的构建功能，也可以解决许多大公司的构建 遭受的性能低下的痛苦.

这也是真正的应对表现疼痛许多大型企业建立患。

多项目构建
待翻)
Gradle's support for multi-project build is outstanding. Project dependencies are first class citizens. We allow you to model the project relationships in a multi-project build as they really are for your problem domain. Gradle follows your layout not vice versa.

Gradle provides partial builds. If you build a single subproject Gradle takes care of building all the subprojects that subproject depends on. You can also choose to rebuild the subprojects that depend on a particular subproject. Together with incremental builds this is a big time saver for larger builds.

Many ways to manage your dependencies
Different teams prefer different ways to manage their external dependencies. Gradle provides convenient support for any strategy. From transitive dependency management with remote Maven and Ivy repositories to jars or dirs on the local file system.

Gradle is the first build integration tool
Ant tasks are first class citizens. Even more interesting, Ant projects are first class citizens as well. Gradle provides a deep import for any Ant project, turning Ant targets into native Gradle tasks at runtime. You can depend on them from Gradle, you can enhance them from Gradle, you can even declare dependencies on Gradle tasks in your build.xml. The same integration is provided for properties, paths, etc ...

Gradle fully supports your existing Maven or Ivy repository infrastructure for publishing and retrieving dependencies. Gradle also provides a converter for turning a Maven pom.xml into a Gradle script. Runtime imports of Maven projects will come soon.

Ease of migration
Gradle can adapt to any structure you have. Therefore you can always develop your Gradle build in the same branch where your production build lives and both can evolve in parallel. We usually recommend to write tests that make sure that the produced artifacts are similar. That way migration is as less disruptive and as reliable as possible. This is following the best-practices for refactoring by applying baby steps.

Groovy
Gradle's build scripts are written in Groovy, not XML. But unlike other approaches this is not for simply exposing the raw scripting power of a dynamic language. That would just lead to a very difficult to maintain build. The whole design of Gradle is oriented towards being used as a language, not as a rigid framework. And Groovy is our glue that allows you to tell your individual story with the abstractions Gradle (or you) provide. Gradle provides some standard stories but they are not privileged in any form. This is for us a major distinguishing features compared to other declarative build systems. Our Groovy support is also not just some simple coating sugar layer. The whole Gradle API is fully groovynized. Only by that using Groovy is the fun and productivity gain it can be.

The Gradle wrapper
The Gradle Wrapper allows you to execute Gradle builds on machines where Gradle is not installed. This is useful for example for some continuous integration servers. It is also useful for an open source project to keep the barrier low for building it. The wrapper is also very interesting for the enterprise. It is a zero administration approach for the client machines. It also enforces the usage of a particular Gradle version thus minimizing support issues.

Free and open source
Gradle is an open source project, and is licensed under the ASL.

