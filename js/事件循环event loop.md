## 事件循环

js从主线程执行代码，遇到宏任务把宏任务放到宏任务消息队列，遇到微任务，放入微任务消息队列，执行完，先把微任务中的所有任务执行完，再取一个宏任务执行，如此往复，形成事件循环

宏任务：

* ui交互
* setTimeout
* setInterval
* I/O

微任务：

* promise
* process.nextTick

### demo

```js
console.log('start')

setTimeout(function() {
  console.log('setTimeout')
}, 0)

Promise.resolve().then(function() {
  console.log('promise1')
}).then(function() {
  console.log('promise2')
})

console.log('end')
// start
// end
// promise1
// promise2
// setTimeout
```

```js
async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end'); //微1
}
async function async2() {
    console.log('async2');
}
console.log('script start');
setTimeout(function() {
    console.log('setTimeout');
}, 0) // 宏1
async1(); 
new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
}); // 微2
console.log('script end');
// script start
// async1 start
// async2
// promise1
// script end
// async1 end
// promise2
// setTimeout
```

```js
console.log('start');
setTimeout(() => {
    console.log('children2');
    Promise.resolve().then(() => {
        console.log('children3');
    }) // 宏1微1
}, 0);// 宏1

new Promise(function(resolve, reject) {
    console.log('children4');
    setTimeout(function() {
        console.log('children5');
        resolve('children6')
    }, 0) // 宏2
}).then((res) => {
    console.log('children7');// 宏2微1
    setTimeout(() => {
        console.log(res);
    }, 0) // 宏3
})// 微1
// start
// children4
// children2
// children3
// children5
// children7
// children6
```

```js
const p = function() {
    return new Promise((resolve, reject) => {
        const p1 = new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve(1)
            }, 0)
            resolve(2)
        })
        p1.then((res) => {
            console.log(res);
        })
        console.log(3);
        resolve(4);
    })
}


p().then((res) => {
    console.log(res);
})
console.log('end');

// 3
// end
// 2
// 4
```

## nodejs事件循环

![node原理](https://user-gold-cdn.xitu.io/2019/11/18/16e7d83cb93ba59e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

* 应用层：js交互层，常见的就是nodejs的模块，比如http，fs
* v8：解析js语法，从而和下层api进行交互
* NodeApi层：与操作系统进行交互
* libuv：实现了事件循环，异步操作等，nodejs实现异步的核心

暂时对nodejs还有理解不深

占坑https://zhuanlan.zhihu.com/p/100090251

http://lynnelv.github.io/js-event-loop-nodejs

https://juejin.cn/post/6844903999506923528