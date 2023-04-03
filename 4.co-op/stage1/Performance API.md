# Performance API

Performance API（性能API）是一种Web API，它允许开发人员测量和分析Web应用程序的性能，例如加载时间、响应时间、资源使用情况等等。它是现代浏览器提供的Web平台API的一部分。


## Performance API 概述

### 1. Performance 对象

> window.performance 是W3C性能小组引入的新的API，目前IE9以上的浏览器都支持。

<img src="https://raw.githubusercontent.com/linhaishe/blogImageBackup/main/performance-api/image-20230403214549741.png" style="zoom:50%;" />

一个performance对象的完整结构包括：

1. memory字段代表JavaScript对内存的占用。
2. navigation字段统计的是一些网页导航相关的数据：
3. redirectCount:重定向的数量（只读），但是这个接口有同源策略限制，即仅能检测同源的重定向；
4. type 返回值应该是0,1,2 中的一个。分别对应三个枚举值:
   - 0 : TYPE_NAVIGATE (用户通过常规导航方式访问页面，比如点一个链接，或者一般的get方式)
   - 1 : TYPE_RELOAD (用户通过刷新，包括JS调用刷新接口等方式访问页面)
   - 2 : TYPE_BACK_FORWARD (用户通过后退按钮访问本页面) 最重要的是timing字段的统计数据，它包含了网络、解析等一系列的时间数据。

### 2. performance.timing 对象

#### 简介

> 🚮**已弃用:** 不再推荐使用该特性。虽然一些浏览器仍然支持它，但也许已从相关的 web 标准中移除，也许正准备移除或出于兼容性而保留。请尽量不要使用该特性，并更新现有的代码；

> ⚠️**警告：** 该属性在 [Navigation Timing Level 2 specification](https://w3c.github.io/navigation-timing/#obsolete) 中已经被废弃，请使用 [`PerformanceNavigationTiming`](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceNavigationTiming) 替代。

`PerformanceTiming` 被废弃了，它的替代对象是 `PerformanceEntry` 和 `PerformanceResourceTiming` 接口。这两个接口提供了与 `PerformanceTiming` 相似的信息，但使用更加灵活和标准化。

- `PerformanceEntry` 接口提供了许多与性能相关的数据，如启动时间、结束时间、持续时间和名称等。
- `PerformanceResourceTiming` 接口继承自 `PerformanceEntry` 接口，提供了更多关于网络资源请求和响应的信息，如 DNS 查询、TCP 连接、SSL/TLS 握手、HTTP 请求和响应等。

这些新的接口提供了更加准确和可靠的性能信息，同时支持跨平台和跨浏览器的使用。

------

#### timing的整体结构

它提供了关于页面加载和渲染时间的详细信息。它包含了与页面生命周期相关的各种时间戳，这些时间戳可以用来测量页面加载的各个阶段以及用户体验。

![image-20230403214930561](https://raw.githubusercontent.com/linhaishe/blogImageBackup/main/performance-api/image-20230403214930561.png)

1. `startTime`：有些浏览器实现为`navigationStart`。该属性返回浏览器开始导航的时间戳，通常是用户输入网址或点击链接的时间。如果当前文档为空，则`navigationStart`的值等于`fetchStart`。
2. `redirectStart`和r`edirectEnd`：如果页面是由`redirect`而来，则`redirectStart`和`redirectEnd`分别代表`redirect`开始和结束的时间节点
3. `unloadEventStart`和u`nloadEventEnd`：如果前一个文档和请求的文档是同一个域的，则`unloadEventStart`和`unloadEventEnd`分别代表浏览器`unload`前一个文档的开始和结束时间节点。否则两者都等于0
4. `fetchStart`是指在浏览器发起任何请求之前的时间值。在`fetchStart`和`domainLookupStart`之间，浏览器会检查当前文档的缓存
5. `domainLookupStart`和`domainLookupEnd`分别代表DNS查询的开始和结束时间节点。如果浏览器没有进行DNS查询（比如使用了cache），则两者的值都等于fetchStart
6. `connectStart`和`connectEnd`分别代表TCP建立连接和连接成功的时间节点。如果浏览器没有进行TCP连接（比如使用持久化连接`webscoket`），则两者都等于`domainLookupEnd`；
7. `secureConnectionStart`：可选。如果页面使用HTTPS，它的值是安全连接握手之前的时刻。如果该属性不可用，则返回`undefined`。如果该属性可用，但没有使用HTTPS，则返回0；
8. `requestStart`代表浏览器发起请求的时间节点，请求的方式可以是请求服务器、缓存、本地资源等；
9. `responseStart`和`responseEnd`分别代表浏览器收到从服务器端（或缓存、本地资源）响应回的第一个字节和最后一个字节数据的时刻；
10. `domLoading`代表浏览器开始解析html文档的时间节点。我们知道IE浏览器下的`document`有`readyState`属性，`domLoading`的值就等于`readyState`改变为`loading`的时间节点；
11. `domInteractive`代表浏览器解析html文档的状态为`interactive`时的时间节点。`domInteractive`并非`DOMReady`，它早于`DOMReady`触发，代表html文档解析完毕（即dom tree创建完成）但是内嵌资源（比如外链css、js等）还未加载的时间点；
12. `domContentLoadedEventStart`：代表`DOMContentLoaded`事件触发的时间节点：
13. 页面文档完全加载并解析完毕之后,会触发`DOMContentLoaded`事件，HTML文档不会等待样式文件,图片文件,子框架页面的加载(load事件可以用来检测HTML页面是否完全加载完毕(fully-loaded))。
14. `domContentLoadedEventEnd`：代表`DOMContentLoaded`事件完成的时间节点，此刻用户可以对页面进行操作，也就是jQuery中的domready时间；
15. `domComplete`：html文档完全解析完毕的时间节点；
16. `loadEventStart和loadEventEnd`分别代表onload事件触发和结束的时间节点

---------

#### 使用注意⚠️

1. 浏览器兼容性：不是所有的浏览器都支持performance.timing接口，因此需要先检查浏览器是否支持该接口。同时，不同浏览器的性能监测方式可能有所不同，可能需要对不同浏览器进行特别处理。
2. 受其他因素影响：页面加载时间受到很多因素的影响，比如网络速度、服务器响应时间、缓存情况等等。因此，通过计算的时间戳之差，得出的只是一个近似值，可能存在误差。
3. DOM操作：计算 `loadEventEnd` 和 `navigationStart` 的时间戳之差并不包括DOM操作时间。如果页面中存在大量的DOM操作，需要考虑这部分时间对页面加载时间的影响，并在计算时进行排除。
4. 避免阻塞：在测量页面加载时间时，应该尽可能避免阻塞其他操作，以便得到更准确的结果。可以使用异步加载等技术来减少阻塞时间。
5. 慎用同步获取：在获取 `performance.timing` 的数据时，需要注意这些数据可能是异步获取的。如果在获取数据之前尝试同步访问这些属性，可能会得到错误的结果。因此，需要使用异步的方式获取 `performance.timing` 的数据。

### 3. Navigation Timing 接口

`PerformanceNavigationTiming` 接口继承自 `PerformanceResourceTiming` 接口，它提供了有关网页导航性能的更详细的信息。`PerformanceNavigationTiming` 对象是通过 `performance.getEntriesByType('navigation')` 方法获取的。

`PerformanceNavigationTiming` 接口提供了以下属性：

通过这些属性，您可以获得有关页面导航过程的详细信息，例如 DNS 查询、TCP 连接、SSL/TLS 握手、HTTP 请求和响应、DOM 解析和渲染等过程的时间戳。这些信息可以用于分析和优化页面性能。

- `unloadEventStart`：返回前一个页面的 `unload` 事件开始的时间戳。
- `unloadEventEnd`：返回前一个页面的 `unload` 事件结束的时间戳。
- `redirectStart`：返回重定向开始时的时间戳。如果没有重定向发生，则返回 0。
- `redirectEnd`：返回重定向结束时的时间戳。如果没有重定向发生，则返回 0。
- `fetchStart`：返回浏览器开始检索当前文档的时间戳。
- `domainLookupStart`：返回浏览器开始执行 DNS 查询的时间戳。
- `domainLookupEnd`：返回浏览器完成 DNS 查询的时间戳。
- `connectStart`：返回浏览器开始连接到服务器的时间戳。
- `connectEnd`：返回浏览器完成连接到服务器的时间戳。
- `secureConnectionStart`：返回浏览器开始安全连接握手的时间戳。如果当前文档不是通过安全连接加载的，则返回 0。
- `requestStart`：返回浏览器向服务器发送请求的时间戳。
- `responseStart`：返回浏览器从服务器接收到第一个字节的时间戳。
- `responseEnd`：返回浏览器从服务器接收到最后一个字节的时间戳。
- `domLoading`：返回浏览器开始解析文档对象模型 (DOM) 的时间戳。
- `domInteractive`：返回浏览器完成解析并生成 DOM 树的时间戳。
- `domContentLoadedEventStart`：返回 DOMContentLoaded 事件开始的时间戳。
- `domContentLoadedEventEnd`：返回 DOMContentLoaded 事件结束的时间戳。
- `domComplete`：返回浏览器完成加载所有子资源的时间戳。
- `loadEventStart`：返回 load 事件开始的时间戳。
- `loadEventEnd`：返回 load 事件结束的时间戳。

### 4. PerformanceEntry 对象

在 Performance API 中，`PerformanceEntry` 对象代表了一个性能条目，它包含了有关某个特定的测量对象（如页面、资源、事件等）的信息。

`PerformanceEntry` 对象本身只是一个用于描述某个性能条目（如资源加载、页面导航等）的数据结构，它存储了该性能条目的各项性能数据，但是并没有提供实际的性能分析工具或方法。因此，`PerformanceEntry` 对象仅仅是一个中间数据格式，不能单独使用。我们通常需要将收集到的 `PerformanceEntry` 对象传递给其他性能分析工具，如 `PerformanceObserver` 或其他第三方性能监测库，才可以对其进行进一步处理和优化。

每个 `PerformanceEntry` 对象都包含了以下共同属性：

- `name`：表示该条目的名称；
- `entryType`：表示该条目的类型，通常是一个字符串，例如 "navigation"、"resource" 或 "measure"；
- `startTime`：表示该条目的起始时间戳；
- `duration`：表示该条目持续的时间。

```ts
[Exposed=(Window,Worker)]
interface PerformanceEntry {
  readonly    attribute DOMString           name;
  readonly    attribute DOMString           entryType;
  readonly    attribute DOMHighResTimeStamp startTime;
  readonly    attribute DOMHighResTimeStamp duration;
  [Default] object toJSON();
};
```

基于不同的类型，`PerformanceEntry` 对象还可能包含其他特定的属性。例如，对于 `Resource Timing` 来说，`PerformanceEntry` 对象包括了以下额外的属性：

- `initiatorType`：表示发起该资源请求的类型，例如 "img"、"script" 等；
- `nextHopProtocol`：表示该资源的传输协议，例如 "http/1.1"、"h2" 等；
- `encodedBodySize`：表示该资源的编码后大小；
- `decodedBodySize`：表示该资源的解码后大小；
- `transferSize`：表示该资源的传输大小。

### 5. PerformanceObserver 对象

`PerformanceObserver` 是一个用于监听和收集 Web 应用程序性能数据的接口，它提供了一种简单但有效的方法来获取各种性能条目的详细信息，并在指定条件满足时发送通知。通过 `PerformanceObserver` 接口，我们可以实时监测应用程序的性能表现，并对其进行分析和优化。

`PerformanceObserver` 接口提供了以下几个重要的方法：

- `PerformanceObserver.observe()`: 用于启动 `PerformanceObserver` 实例，并设置它要监听的指定类型的性能条目。该方法接受一个配置对象作为参数，其中包括要监听的性能条目类型、回调函数等。
- `PerformanceObserver.disconnect()`: 用于停止 `PerformanceObserver` 实例的监听操作，并释放其中的资源。
- `PerformanceObserver.takeRecords()`: 用于获取当前已经被收集到的所有性能条目，并清空原有的记录缓存。该方法返回一个 `PerformanceEntryList` 对象，其中包含所有被收集到的性能条目。

https://codepen.io/linhaishe/pen/MWqNQOJ?editors=0111

## 3. 计算性能指标

### a. 计算页面加载时间

通过计算`loadEventEnd`和`navigationStart`的时间戳之差，可以得出页面的加载时间。

这个时间包括了所有的资源加载时间和DOM操作时间。

```js
var pageLoadTime = performance.timing.loadEventEnd - performance.timing.navigationStart;
console.log('页面加载时间为：' + pageLoadTime + ' 毫秒');
```

https://codepen.io/linhaishe/pen/BaOXaGw?editors=0011

例子的loadEventEnd跑出来是0？？？

**`PerformanceTiming.loadEventEnd`** 是一个返回代表一个时刻的 `unsigned long long` 型只读属性，为 [`load`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/load_event) 事件处理程序被终止，加载事件已经完成之时的 Unix 毫秒时间戳。如果这个事件没有被触发，或者没能完成，则该值将会返回 `0`。

### b. 优化DOM操作

通过计算`domContentLoadedEventEnd`和`domContentLoadedEventStart`的时间戳之差，可以得出DOM操作的时间。

可以根据DOM操作的时间来优化代码，减少DOM操作的次数或使用更高效的操作方法，以提高页面性能。

```js
var domLoadingTime = performance.timing.domContentLoadedEventEnd - performance.timing.domContentLoadedEventStart;
console.log('DOM操作时间为：' + domLoadingTime + ' 毫秒');
```

https://codepen.io/linhaishe/pen/OJoKJvm?editors=1112

### c. 计算资源加载时间

通过计算`responseEnd`和`fetchStart`的时间戳之差，可以得出资源加载时间。

可以根据资源加载的时间来调整资源的加载顺序和使用方法，以提高页面性能和用户体验。

```js
var resourceLoadTime = performance.timing.responseEnd - performance.timing.fetchStart;
console.log('资源加载时间为：' + resourceLoadTime + ' 毫秒');
```

https://codepen.io/linhaishe/pen/zYJgEmR?editors=0011













## 其他内容

Performance API（性能API）是一种Web API，它允许开发人员测量和分析Web应用程序的性能。它是现代浏览器提供的Web平台API的一部分。

通过Performance API，开发人员可以测量Web应用程序的各个方面的性能，例如加载时间、响应时间、资源使用情况等等。Performance API提供了各种方法和指标，以便开发人员可以识别和解决性能瓶颈和问题，并提高应用程序的性能和用户体验。

一些常用的Performance API方法和指标包括：

- performance.timing：测量页面加载和各种事件的时间。
- performance.now()：提供高精度的时间戳，用于测量代码执行时间。
- performance.mark()和performance.measure()：允许开发人员标记和测量应用程序中的特定时间间隔。
- PerformanceObserver API：提供了一种方法来监视性能指标并在达到特定阈值时触发通知。

## 

测量页面加载和各种事件的时间

1. 检测页面加载时间：使用Navigation Timing API获取页面开始加载和页面完成加载的时间，计算它们之间的差值，即可得到页面加载时间。可以使用此信息来确定哪些资源加载缓慢，并尝试优化它们。

   ```js
   var pageLoadTime = performance.timing.loadEventEnd - performance.timing.navigationStart;
   console.log('Page load time: ' + pageLoadTime + 'ms');
   ```

2. 监测资源加载时间：使用Resource Timing API获取每个资源的加载时间，并使用此信息来优化资源的大小、缓存策略和服务器响应时间。可以监控每个资源（例如图像、CSS文件和JavaScript文件）的加载时间，并识别加载时间较长的资源。例如，可以使用`performance.getEntriesByType('resource')`方法获取所有资源的详细计时信息

   ```js
   var resources = performance.getEntriesByType('resource');
   resources.forEach(function(resource) {
     console.log(resource.name + ' load time: ' + resource.duration + 'ms');
   });
   ```

3. 检测用户操作响应时间：使用User Timing API创建自定义性能度量标准，例如标记特定代码块的开始和结束时间，并计算它们之间的差值，以确定用户操作的响应时间。

4. 分析代码性能：使用User Timing API创建性能度量标准，例如标记代码块的开始和结束时间，并记录这些标记的时间戳，以便进行更深入的分析。

   ```js
   performance.mark('start');
   // 执行一些代码块
   performance.mark('end');
   performance.measure('Code block execution time', 'start', 'end');
   var measure = performance.getEntriesByName('Code block execution time')[0];
   console.log('Code block execution time: ' + measure.duration + 'ms');
   ```

5. 优化动画性能：使用High Resolution Time API获取高分辨率的时间戳，并使用它们来创建平滑的动画效果，避免视觉抖动和卡顿。

   ```js
   const t0 = performance.now();
   
   // 执行一些耗时的代码
   
   const t1 = performance.now();
   console.log(`执行耗时为 ${t1 - t0} 毫秒`);
   ```

6. 分析事件性能：使用Performance Timeline API获取有关浏览器事件（例如JavaScript执行、CSS解析、重绘等）的详细计时信息，并使用此信息来诊断性能问题。

   Performance Timeline API 包括多个相关的接口，常用的有以下几个：

   1. PerformanceEntry 接口：代表了一个性能记录条目，其中包含了某个性能事件的相关信息，比如名称、开始时间、结束时间等。

   2. PerformanceObserver 接口：用于观察性能条目的变化，当有新的性能条目被添加时，可以收到通知并进行处理。

   3. Performance 对象：代表了当前页面的性能信息，包含了各种性能指标，比如页面加载时间、资源加载时间、响应时间等。

      ```js
      const observer = new PerformanceObserver((list) => {
        const entries = list.getEntries();
        for (const entry of entries) {
          if (entry instanceof PerformanceResourceTiming) {
            console.log(`资源 ${entry.name} 的加载时间为 ${entry.duration} 毫秒`);
          }
        }
      });
      
      observer.observe({ entryTypes: ["resource"] });
      
      ```

      Performance Timeline API 可能会影响浏览器的性能，因此在使用时应该谨慎。如果只需要获取简单的性能指标，可以考虑使用 Performance 对象提供的方法，比如 `performance.timing`、`performance.navigation` 等。
