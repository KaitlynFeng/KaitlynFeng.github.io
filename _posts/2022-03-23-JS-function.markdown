---
layout: post
title:  "JS 函数的执行时机"
date:   2022-03-09 14:58:26 +0800
categories: javascript
---


 # 我们可以用以下代码来分析JS的执行时机：

```javascript
let i 
for( i = 0; i<6; i++){
    setTimeout(()=>{
        console.log(i)
    },0)
}
//输出6个6 
```
for循环执行步骤：

1、i赋值为0

2、判断i < 6 ?,满足进入第一循环

3、setTimeout()会过一会执行–>跳过setTimeout()继续执行

4、执行i++，此时i的值为1，判断i < 6 ?,满足进入第二循环

省略…

5、执行i++，此时i的值为6

6、判断i < 6 ?,不满足跳出循环

 * 执行第一次循环的setTimeout() //打印出a
 * 执行第二次循环的setTimeout() //打印出a
 * 执行第三次循环的setTimeout() //打印出a
 * 执行第四次循环的setTimeout() //打印出a
 * 执行第五次循环的setTimeout() //打印出a
 * 执行第六次循环的setTimeout() //打印出a
 * 
结束

 # setTimeout()的'过一会'执行究竟是多久呢？
 
 ```setTimeout(fn,0)```的含义是，指定某个任务在主线程最早可得的空闲时间执行，意思就是不用再等多少秒了，只要主线程执行栈内的同步任务全部执行完成，栈为空就马上执行。也就是当同步任务的函数和语句执行完后，0秒或者立刻执行setTimeout(fn,0)。

 # 输出0到5
 
 ```javascript
 for(let i = 0; i<6; i++){
    setTimeout(()=>{
        console.log(i)
    },0)
}
// 0 1 2 3 4 5
```

 因为```let```变量的作用域只能在当前函数中，所以每次for循环生成的都是一个新的i， setTimeout里输出的i就是这个新的i，这个i是不会变化的，所以输出的就是正常的。
 
 # 其他方法
 
 ```javascript
 let i 
for(i = 0; i<6; i++){
  !function(j){
      setTimeout(()=>{
        console.log(j)
      },0)
  }(i)
}
 ```
 
