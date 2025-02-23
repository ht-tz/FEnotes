# 事件循环：宏任务、微任务

~~`JavaScript`是一门单线程的语言，意味着同一时间内只能做一件事，但是这并不意味着单线程就是阻塞，而实现单线程非阻塞的方法就是事件循环~~

**Event Loop:** An Event Loop in JavaScript is said to be a constantly running process which keeps a tab on the call stack. Its main function is to check whether the call stack is empty or not. If the call stack turns out to be empty, the event loop proceeds to execute all the callbacks waiting in the task queue. Inside the task queue, the tasks are broadly classified into two categories, namely micro-tasks and macro-tasks.

JavaScript 中的事件循环是一个持续运行的过程，它不断监听call stack（调用栈）。它的主要功能是检查调用栈是否为空。如果调用栈为空，事件循环继续执行任务队列中等待的所有回调。在任务队列中，任务大致分为两类，即微任务和宏任务

宏任务：setTimeout, setInterval, setImmediate, requestAnimationFrame, I/O, UI Rendering

微任务：Promise, async/await, process.nextTick, Promises, queueMicrotask, MutationObserver

![eventloop](https://raw.githubusercontent.com/linhaishe/blogImageBackup/main/event-loop/eventloop.gif)

![image-20230325195936478](https://raw.githubusercontent.com/linhaishe/blogImageBackup/main/event-loop/image-20230325195936478.png)

## 1. 同步/宏/微任务的执行顺序

> 执行顺序：同步任务 > 微任务 > 宏任务

```js
setTimeout(() => {
  console.log("宏任务");
}, 0);

Promise.resolve().then((res) => {
  console.log("微任务");
});

console.log("同步任务");
```

## 2. 定时器的任务编排

**定时器任务会根据主线任务处理完之后才会开始执行，即使定时器时间为0**

```js
// 倒计时会有一个最短计时，即使设置为0毫秒后执行，js也会默认给上一个4毫秒的延迟在执行
// 所以0毫秒的倒计时不是真正的0毫秒倒计时

setTimeout(() => {
  console.log("倒计时");
}, 0);

for (let a = 0; a < 10000; a++) {
  console.log("");
}
```

![image-20230325200033496](https://raw.githubusercontent.com/linhaishe/blogImageBackup/main/event-loop/image-20230325200033496.png)

**倒计时任务会根据时间长短进行顺序输出**

浏览器在正常解析式遇到定时器会把定时器放在定时器的队列中去开始计时，等待倒计时结束后会把任务放在宏任务队列中，等待同步任务执行完毕后直接调用宏任务队列中的方法执行，**定时器时间进入宏任务后， 如果有多个倒计时，则那个倒计时先计时完毕会先优先进入宏任务队列中**

```js
setTimeout(() => {
  console.log("倒计时1");
}, 2000);

setTimeout(() => {
  console.log("倒计时2");
}, 1000);

for (let a = 0; a < 10; a++) {
  console.log("");
}

// '' -> 倒计时2 -> 倒计时1
```

![image-20230325200053734](https://raw.githubusercontent.com/linhaishe/blogImageBackup/main/event-loop/image-20230325200053734.png)

**多个倒计时会同时输出**。

倒计时任务不是指等它上一个任务结束后才开始倒计时，而是在进入定时器任务模块中就开始进行倒计时，当其进入宏任务后，会直接输出，而不会再重新进行倒计时。如果宏任务队列中有等待执行的代码就拿过来执行，否则就进行休眠等待下一次解析。

```js
setTimeout(() => {
  console.log("倒计时1");
}, 2000);

setTimeout(() => {
  console.log("倒计时2");
}, 1000);

setTimeout(() => {
  console.log("倒计时");
}, 0);

for (let a = 0; a < 10; a++) {
  console.log("");
}
// 执行后可以看到等待10次循环完毕后不是再等待2秒打印结果，而是同时打印出倒计时2和倒计时1
```

## 3. promise 微任务处理

> 运行结果: promise –> 同步任务 –> then –> settimeout

```js
setTimeout(() => {
  console.log("settimeout");
}, 0);

new Promise((resolve, reject) => {
  console.log("promise");
  resolve();
}).then((res) => {
  console.log("then");
});

console.log("同步任务");
```

- 主进程遇到settimeout则开始倒计时，放入定时器任务模块，倒计时完毕后把任务放在宏任务队列中。
- promise中执行器（构造函数）属于同步任务，所以先打印出promise。
- 然后遇到then，promise返回状态后进入微任务队列，同样等待同步任务执行
- 紧接着遇到同步任务打印出同步任务
- 同步任务执行完毕后执行微任务打印then
- 微任务执行完毕后执行宏任务打印settimeout

Promise中的执行器（构造函数）属于==同步任务==。then()是异步函数，属于微任务。

**Promise 构造函数: new Promise(executor)**

- executor 函数: 执行器 (resolve, reject) = {}

- resolve 函数: 内部定义成功时我们调用的函数 value = {} 

- reject 函数: 内部定义失败时我们调用的函数 reason = {} 

```js
const myFirstPromise = new Promise((resolve, reject) => {
  // do something asynchronous which eventually calls either:
  //   resolve(someValue)        // fulfilled
  // or
  //   reject("failure reason")  // rejected
});
```

说明: executor 会在 Promise ==内部立即**同步调用**==,异步操作在执行器中执行,换话说Promise支持同步也支持异步操作

## 4. DOM 渲染任务

一般要把 js 放在页面底部处理，这样不会因为等待 js 执行而导致页面白屏等待

```html
<html lang="en">
	<head>
 	 <meta charset="UTF-8" />
 	 <meta name="viewport" content="width=device-width, initial-scale=1.0" />
 	 <title>Document</title>
	</head>
	<!-- 如果js放在DOM前面，则会等待js业务逻辑处理完毕后再去渲染DOM，会导致页面在js处理期间显示白屏，造成体验不友好，所以通常把js模块放在页面底部 -->
	<!-- <script src="./js/1.js"></script> -->
	<body>
 	 <h2>hello,world</h2>
	</body>
	<script src="./js/1.js"></script>
</html>
```

**执行顺序：微任务 -> DOM渲染 -> 宏任务**

```js
const $content = $("<p>内容</p>");
$("#box").append($content);

console.log(1);

Promise.resolve().then(() => {
  console.log("2 promise");
  // alert('promise then' );
});

setTimeout(() => {
  console.log("3 setTimeout");
  // alert('setTimeout');
}, 0);

console.log(4);

// 1 -> 4 -> 2 promise -> Dom渲染 -> 3 setTimeout
```

## 5. 任务共享内存

> 运行结果：经过一秒后打印出 1 和 2

```js
let i = 0;

setTimeout(() => {
  console.log(++i);
}, 1000);

setTimeout(() => {
  console.log(++i);
}, 1000);
```

解析：js解析到第一个计时器后会把第一个计时器放在宏任务队列中，然后接着把第二个计时器放在宏任务队列中，同步任务执行完毕后去宏任务队列获取任务，第一个任务执行完毕后改变了i的值，此时i的值变成了1，然后执行第二个任务，经过上个任务的处理，i的值已经是1，在经过++变成2，所以打印结果为倒计时1秒后打印出 1  2。

![image-20230402165544675](https://raw.githubusercontent.com/linhaishe/blogImageBackup/main/coop/image-20230402165544675.png)

## 6. 任务轮询之进度条

```css
.progressBar {
  height: 30px;
  background-color: aquamarine;
  position: absolute;
  text-align: center;
}
```

```html
<div class="progressBar"></div>
```

```js
function progre() {
  let i = 0;
  let div = document.querySelector(".progressBar");
  (function run() {
    if (++i <= 100) {
      div.innerHTML = i;
      div.style.width = i + "%";
      setTimeout(run, 10);
    }
  })();
}
progre();

// 代码解析：先执行progre方法，方法内部存在自执行函数run，run方法会产生一个计时器放在
// 定时器会生成宏任务进入到宏任务队列中，计时器调用run方法重复生成新的宏任务
// 循环往复会产生多个宏任务，if判断i的值小于等于100时不再产生新的宏任务
```

## 7. 任务拆分成多个子任务

```css
.box {
  width: 300px;
  height: 20px;
  position: relative;
  border: 1px solid;
}

.progre {
  position: absolute;
  height: 100%;
  width: 0%;
  background-color: #09c;
}
```

```html
<div class="box">
  <div class="progre"></div>
</div>
<div class="progretext">0</div>
<button onclick="hd()">开始下载</button>
```

```js
let num = 984755554;
let nums = 984755554;
let count = 0;
let progrediv = document.querySelector(".progre");
let progretext = document.querySelector(".progretext");

function hd() {
  // 每次先循环一部分
  for (let i = 0; i < 1000000; i++) {
    if (num <= 0) break;
    count = num--;
  }
  // 循环完后判断是否还有值未参与循环
  // 如果大于0表示该有值未参与循环，则放到宏任务中执行后续循环
  // 不影响下面的同步任务继续执行
  if (num > 0) {
    setTimeout(hd);
  }

  let progrs = (100 - (num / nums) * 100).toFixed(2) + "%";
  progrediv.style.width = progrs;
  progretext.innerHTML = progrs;

  if (num === 0) {
    progrediv.style.background = "#5edc63";
  }
  console.log(progrs);
}

```

## 8. promise 微任务处理复杂任务

```js
// 利用异步执行先执行完同步任务后再去执行微任务
async function hd(num) {
  let res = await Promise.resolve().then((_) => {
    let count = 0;
    for (let i = 0; i < num; i++) {
      count += num--;
    }
    return count;
  });
  console.log(res);
}
hd(456845789);
console.log("同步任务");

// 打印结果
// 同步任务   78265528430191060
```

## 9. `queueMicrotask`

如果我们想要异步执行（在当前代码之后）一个函数，但是要在更改被渲染或新事件被处理之前执行，那么我们可以使用 `queueMicrotask` 来对其进行安排（schedule）。

这是一个与前面那个例子类似的，带有“计数进度条”的示例，但是它使用了 `queueMicrotask` 而不是 `setTimeout`。你可以看到它在最后才渲染。就像写的是同步代码一样：

```html
<div id="progress"></div>

<script>
let i = 0;

function count() {
  // 做繁重的任务的一部分 (*)
  do {
    i++;
    progress.innerHTML = i;
  } while (i % 1e3 != 0);

  if (i < 1e6) {
    queueMicrotask(count);
  }
}

count();

</script>
```

## 10. Web Workers

对于不应该阻塞事件循环的耗时长的繁重计算任务，我们可以使用 [Web Workers](https://html.spec.whatwg.org/multipage/workers.html)。

这是在另一个并行线程中运行代码的方式。

Web Workers 可以与主线程交换消息，但是它们具有自己的变量和事件循环。

Web Workers 没有访问 DOM 的权限，因此，它们对于同时使用多个 CPU 内核的计算非常有用。

## 11. refs

1. [javascript.info](https://zh.javascript.info/event-loop#shi-jian-xun-huan)

3. [SZX 的开发笔记](https://szxio.gitee.io/hexoblog/JavaScript/MacroTask/)

4. [loop playground](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)

5. [✨♻️ JavaScript Visualized: Event Loop](https://dev.to/lydiahallie/javascript-visualized-event-loop-3dif)
6. [geeksforgeeks](https://www.geeksforgeeks.org/what-are-the-microtask-and-macrotask-within-an-event-loop-in-javascript/)

