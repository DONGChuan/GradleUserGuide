# 属性存取器
Groovy 自动将一个属性的引用转换为相应的 getter 或 setter 方法.

**例子: 13.5. 属性存取器**

    // 使用 getter 方法
    println project.buildDir
    println getProject().getBuildDir()

    // 使用 setter 方法
    project.buildDir = 'target'
    getProject().setBuildDir('target')


