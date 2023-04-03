# Performance API

Performance API（性能API）是一种Web API，它允许开发人员测量和分析Web应用程序的性能，例如加载时间、响应时间、资源使用情况等等。它是现代浏览器提供的Web平台API的一部分。

## 1. performance.timing对象

> 🚮**已弃用:** 不再推荐使用该特性。虽然一些浏览器仍然支持它，但也许已从相关的 web 标准中移除，也许正准备移除或出于兼容性而保留。请尽量不要使用该特性，并更新现有的代码；

> ⚠️**警告：** 该属性在 [Navigation Timing Level 2 specification](https://w3c.github.io/navigation-timing/#obsolete) 中已经被废弃，请使用 [`PerformanceNavigationTiming`](https://developer.mozilla.org/zh-CN/docs/Web/API/PerformanceNavigationTiming) 替代。

`PerformanceTiming` 被废弃了，它的替代对象是 `PerformanceEntry` 和 `PerformanceResourceTiming` 接口。这两个接口提供了与 `PerformanceTiming` 相似的信息，但使用更加灵活和标准化。

- `PerformanceEntry` 接口提供了许多与性能相关的数据，如启动时间、结束时间、持续时间和名称等。
- `PerformanceResourceTiming` 接口继承自 `PerformanceEntry` 接口，提供了更多关于网络资源请求和响应的信息，如 DNS 查询、TCP 连接、SSL/TLS 握手、HTTP 请求和响应等。

这些新的接口提供了更加准确和可靠的性能信息，同时支持跨平台和跨浏览器的使用。

------

它提供了关于页面加载和渲染时间的详细信息。它包含了与页面生命周期相关的各种时间戳，这些时间戳可以用来测量页面加载的各个阶段以及用户体验。

1. navigationStart：该属性返回浏览器开始导航的时间戳，通常是用户输入网址或点击链接的时间。
2. fetchStart：该属性返回浏览器开始请求文档的时间戳，通常是在地址栏中输入网址并按下回车键的时间。
3. responseEnd：该属性返回浏览器接收到文档的时间戳，即文档下载完成的时间。
4. domLoading：该属性返回浏览器开始解析文档的时间戳，即浏览器开始构建DOM树的时间。
5. domInteractive：该属性返回浏览器完成解析文档的时间戳，即DOM树构建完成的时间。
6. domContentLoadedEventStart：该属性返回浏览器开始解析文档中的脚本和CSS的时间戳。
7. domContentLoadedEventEnd：该属性返回浏览器完成解析文档中的脚本和CSS的时间戳。
8. loadEventStart：该属性返回浏览器开始加载页面中的所有资源（如图像和脚本）的时间戳。
9. loadEventEnd：该属性返回浏览器完成加载所有资源的时间戳。

注意⚠️

1. 浏览器兼容性：不是所有的浏览器都支持performance.timing接口，因此需要先检查浏览器是否支持该接口。同时，不同浏览器的性能监测方式可能有所不同，可能需要对不同浏览器进行特别处理。
2. 受其他因素影响：页面加载时间受到很多因素的影响，比如网络速度、服务器响应时间、缓存情况等等。因此，通过计算的时间戳之差，得出的只是一个近似值，可能存在误差。
3. DOM操作：计算 `loadEventEnd` 和 `navigationStart` 的时间戳之差并不包括DOM操作时间。如果页面中存在大量的DOM操作，需要考虑这部分时间对页面加载时间的影响，并在计算时进行排除。
4. 避免阻塞：在测量页面加载时间时，应该尽可能避免阻塞其他操作，以便得到更准确的结果。可以使用异步加载等技术来减少阻塞时间。
5. 慎用同步获取：在获取 `performance.timing` 的数据时，需要注意这些数据可能是异步获取的。如果在获取数据之前尝试同步访问这些属性，可能会得到错误的结果。因此，需要使用异步的方式获取 `performance.timing` 的数据。

------------

### a. 计算页面加载时间

通过计算`loadEventEnd`和`navigationStart`的时间戳之差，可以得出页面的加载时间。

这个时间包括了所有的资源加载时间和DOM操作时间。

```js
var pageLoadTime = performance.timing.loadEventEnd - performance.timing.navigationStart;
console.log('页面加载时间为：' + pageLoadTime + ' 毫秒');
```

https://codepen.io/linhaishe/pen/BaOXaGw?editors=0012

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

## 2. PerformanceNavigationTiming

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

