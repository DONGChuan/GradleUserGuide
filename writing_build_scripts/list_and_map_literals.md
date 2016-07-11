#  List 和 Map 集合
Groovy 为预定义的 List  和 Map 集合提供了一些操作捷径，这两个字面值都比较简单易懂，但是 Map 会有一些不同.

例如，当您使用 "apply" 方法使用插件时，apply 会自动加上 Map 的一个参数，当您这样写 " apply plugin: 'java' "时，实际上使用的是 name 参数(name-value)，只不过在 Groovy 中 使用 Map 没有 < > ,当方法被调用的时候，name 参数就会被转换成 Map 键值对，只不过在 Groovy 中看起来不像一个 Map.

**例子 13.7.List 和 Map 集合

**build.gradle**

    // List 集合
    test.includes = ['org/gradle/api/**', 'org/gradle/internal/**']

    List<String> list = new ArraryList<String>()
    list.add('org/gradle/api/**')
    list.add('org/gradle/internal/**')
    test.includes = list

    // Map 集合
    Map<String,String> map = [key1:'value1', key2:'valu2']

    // Groovy 会强制将Map的键值对转换为只有value的映射
    apply plugin: 'java'

