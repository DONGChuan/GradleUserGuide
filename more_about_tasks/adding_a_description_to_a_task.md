# 给 task 加入描述

你可以给你的任务加入一段描述性的文字. 它将会在任务执行的时候显示出来.

**例子 15.18. 给任务加入描述**

**build.gradle**

```
task copy(type: Copy) {
   description 'Copies the resource directory to the target directory.'
   from 'resources'
   into 'target'
   include('**/*.txt', '**/*.xml', '**/*.properties')
}
```
