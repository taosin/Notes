# ES6之Promise简单解读
### 1. Promise 的含义

所谓 `Promise` ，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，`Promise` 是一个对象，从它可以获取异步操作的消息。`Promise` 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

### 2. Promise 的特点：

它有以下两个特点：

#### （1）对象的状态不受外界影响。 `Promise` 对象代表一个异步操作，有三种状态：`pending` （进行中）、`fulfilled`（已成功）和 `rejected` （已失败）。*只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。*

#### （2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。 `Promise` 对象的状态改变，只有两种可能：从 `pending` 变为 `fulfilled` 和 从 `pending` 变为 `rejected` 。 *如果改变已经发生了，你再对 `Promise` 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。*

#### 3. Promise 的优点

（1）`Promise` 对象可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数（即 ~~回调地狱~~ ）。
（2）`Promise` 对象提供统一的接口，使得控制异步操作更加容易。

#### 4. Promise 的缺点
（1）`Promise` 一旦被新建就会立即执行，无法中途取消。
  (2)  如果不设置回调函数， `Promise` 内部抛出的错误，则不会反应到外部。
（3）当处于 `pending` 状态时，无法得知目前 `Promise` 进展到哪一个阶段（刚刚开始还是即将完成）。

- - - -
### 基本用法

ES6规定， `Promise` 对象是一个构造函数，用来生成 `Promise` 实例。
```js
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

`Promise` 构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve` 和 `reject`。它们是两个函数，由 **JavaScript** 引擎提供，不用自己部署。

`resolve` 函数的作用是，将 `Promise` 对象的状态从“未完成”变为“成功”（即从 `pending` 变为 `resolved`），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject` 函数的作用是，将`Promise` 对象的状态从“未完成”变为“失败”（即从 `pending` 变为 `rejected`），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

`Promise` 实例生成以后，可以用 `then` 方法分别指定 `resolved` 状态和 `rejected` 状态的回调函数。

```js
promise.then(function(value)){
	// success
}, function(err){
	// failure	
});
```
`then` 方法可以接受两个回调函数作为参数。第一个回调函数是 `Promise` 对象的状态变为 `resolved` 时调用，第二个回调函数是`Promise` 对象的状态变为 `rejected` 时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受 `Promise` 对象传出的值作为参数。

#### 示例：

* `Promise`  定时执行 ：

```js
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}

timeout(100).then((value) => {
  console.log(value);
});
```

上述代码中，`timeout` 方法返回一个 `Promise` 实例， 表示一段时间以后才会发生的结果。过了指定的时间（ `ms` 参数）以后，`Promise` 实例的状态变为 `resolved`，就会触发 `then` 方法绑定的回调函数。

* `Promise` 立即执行：

```js
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved
```

上面代码中，`Promise` 新建后立即执行，所以首先输出的是 `Promise` 。然后，`then` 方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以 `resolved` 最后输出。

* 异步加载图片：

```js
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    var image = new Image();

    image.onload = function() {
      resolve(image);
    };

    image.onerror = function() {
      reject(new Error('Could not load image at ' + url));
    };

    image.src = url;
  });
}
```

注意：调用 `resolve` 或 `reject`并不会终结 `Promise` 的参数函数的执行。

```js
new Promise((resolve, reject) => {
  resolve(1);
  console.log(2);
}).then(r => {
  console.log(r);
});
// 2
// 1
```

上面代码中，调用 `resolve(1)`以后，后面的 `console.log(2)` 还是会执行，并且会首先打印出来。这是因为立即 `resolved` 的 `Promise` 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

一般来说，调用 `resolve` 或 `reject` 以后，`Promise` 的使命就完成了，后继操作应该放到 `then` 方法里面，而不应该直接写在 `resolve` 或 `reject`的后面。所以，最好在它们前面加上 `return`语句，这样就不会有意外。

```js
new Promise((resolve, reject) => {
  return resolve(1);
  // 后面的语句不会执行
  console.log(2);
})
// 1
// 2
```
