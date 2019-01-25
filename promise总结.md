## promise概述
- 基本语句 resolve,reject,then,catch
- 三种状态:pending,fulfilled,rejected
---
### 基本用法
- promise初始化
```
var promise = new Promise(function(resolve,reject)
    resolve('test')
    reject('error')//或者下面这种
    throw 'error' //推荐
);
```

- then,catch用法
```
promise.then(function(data) {
    console.log('success');
}).catch(function(error) {
    console.log('error', error);
});
```
---
### 不常用的方法
- all用法
```
var p = Promise.all([p1,p2,p3])
```
概述:全部都是fulfilled,才执行then,一个是rejected就catch.

**重点**
执行顺序是按参数顺序,不是会回调顺序,并行执行

- race用法

代码同all,有一个状态改变就执行

- 便捷用法
```
Promise.resolve(value)
Promise.reject(reason)
```
---
### 常见问题
1. reject和catch区别
```
var promise = new Promise(function (resolve, reject) {
     reject("333")
     resolve(x);
});

promise.then((value) => {
    console.log(value)
}).catch((error) => {
    
});

// promise.then(function (data) {
//     console.log(data)
// }, function (error) {
//     console.log(error)
//     });

```
**(待议)**
then中的异常catch也能捕获,上文中第二种方式收不到resolve(x)的异常

2. 如果在then中抛错，而没有对错误进行处理（即catch），那么会一直保持reject状态，直到catch了错误.

```
function taskA() {
    console.log(x);
    console.log("Task A");
}
function taskB() {
    console.log("Task B");
}
function onRejected(error) {
    console.log("Catch Error: A or B", error);
}
function finalTask() {
    console.log("Final Task");
}
var promise = Promise.resolve();
promise
    .then(taskA)
    .then(taskB)
    .catch(onRejected)
    .then(finalTask);
    
-------output-------
Catch Error: A or B,ReferenceError: x is not defined
Final Task

```




