# 应用插件

插件需要声明*被应用*,通过[Project.apply()](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html#org.gradle.api.Project:apply(java.util.Map)方法完成.应用的插件是*idempotent*<sup>[注1]()</sup>,即相同的插件可以应用多次.如果插件先前以被应用,任何后来的应用是安全的,不会有任何影响的.


[1]译注:英文直接翻译的意思是幂等(denoting an element of a set that is unchanged in value when multiplied or otherwise operated on by itself.),上下中的大意应该是不会受其他因素的影响.
