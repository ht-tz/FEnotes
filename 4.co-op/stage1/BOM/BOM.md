# BOM介绍

## 1. 什么是BOM

在JavaScript中，BOM是指“Browser Object Model”，即浏览器对象模型。它是JavaScript与浏览器交互的接口，提供了一组用于操作浏览器窗口、文档、历史记录、位置等浏览器相关对象的API。如浏览器历史记录、浏览器窗口大小、屏幕显示、定时器、Cookie等。

## 2. BOM的作用和用途

BOM的作用和用途主要有以下几点：

a. 操作浏览器窗口：BOM提供了可以通过JavaScript代码控制浏览器窗口大小、位置、状态栏、工具栏等属性的API。

b. 操作浏览器文档：BOM提供了可以通过JavaScript代码访问和操作当前文档的API，例如获取和修改文档内容、创建和删除文档元素等。

c. 操作浏览器历史记录：BOM提供了可以通过JavaScript代码操作浏览器历史记录的API，例如在历史记录中前进或后退、改变当前页面的URL等。

d. 操作浏览器位置：BOM提供了可以通过JavaScript代码获取和设置当前页面的位置信息的API，例如获取当前页面的经纬度、获取当前页面的地址等。

## 3. BOM的优缺点

优点：

1. BOM 提供了丰富的浏览器操作和信息获取功能，开发者可以通过 BOM API 来控制浏览器窗口、历史记录、定时器、屏幕显示等非标准化对象。
2. BOM 的 API 可以方便地获取浏览器的相关信息，比如浏览器的类型、版本、语言、分辨率等，可以帮助开发者更好地优化页面。
3. BOM 是浏览器提供的对象模型，与 DOM（文档对象模型）相互配合，可以实现浏览器与页面的交互，如表单提交、AJAX 等操作。
4. 提供了丰富的浏览器操作和信息获取功能，使得开发者能够更加灵活地控制浏览器行为。
5. 可以通过BOM操作浏览器窗口大小和位置，实现一些特定的需求。
6. 提供了操作浏览器历史记录的方法，实现浏览器前进和后退等功能。
7. 提供了定时器的方法，可以按照一定时间间隔执行特定的代码。
8. 可以获取浏览器的信息，如浏览器类型、版本、语言、分辨率等。

缺点：

a. BOM的API在不同浏览器之间存在差异，因此编写跨浏览器的代码需要额外的注意。

b. BOM的API对浏览器的性能和安全性有一定的影响，不当使用可能导致性能问题和安全漏洞。

c. BOM的API不包含对JavaScript语言本身的扩展，因此不适用于非浏览器环境下的JavaScript应用程序。

d. BOM的某些功能容易被滥用，从而导致页面性能问题或者安全问题。

e. BOM只能在浏览器环境中使用，不能在其他环境（如Node.js）中使用。

# window对象

BOM (浏览器对象模型) 中的 `window` 对象是 JavaScript 中的全局对象，window对象表示整个浏览器窗口，它包含了许多属性和方法，如document、navigator、history、setTimeout等。

虽然每个窗口或标签页都有一个对应的window对象，但是不同窗口或标签页之间的window对象是相互独立的，它们之间不能直接访问和操作。如果需要在不同窗口或标签页之间进行通信，可以使用一些技术，如消息传递、共享数据存储等。

1. 全局作用域：`window` 对象包含了所有 JavaScript 全局变量和函数。在浏览器中定义的全局变量和函数都是 `window` 对象的属性和方法。
2. 窗口控制：`window` 对象提供了控制当前浏览器窗口或标签页的方法和属性。例如，可以使用 `window.open()` 方法打开一个新的浏览器窗口。
3. 文档控制：`window` 对象提供了与当前文档交互的方法和属性。例如，可以使用 `window.document` 属性来访问当前文档对象。
4. 定时器：`window` 对象提供了 `setTimeout()` 和 `setInterval()` 方法，可以用来执行一些异步操作，例如延迟执行某个函数。
5. 浏览器信息：`window` 对象提供了许多属性和方法，用于获取浏览器信息，例如浏览器名称、版本和分辨率等。

- window对象的属性和方法

  [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window#%E5%B1%9E%E6%80%A7)

- window对象控制浏览器窗口

  - window.open(url, name, specs, replace)：打开一个新窗口，并返回该窗口的引用。
  - window.close()：关闭当前窗口。
  - window.resizeTo(width, height)：调整窗口大小到指定的宽度和高度。
  - window.moveTo(x, y)：将窗口移动到指定的屏幕坐标位置。
  - window.location.href：获取或设置当前窗口的URL地址。
  - window.document：获取当前窗口所包含的文档对象。

- window对象控制浏览器的导航行为

  - window.location.href：获取或设置当前窗口的URL地址。
  - window.location.assign(url)：将当前窗口的URL地址设置为指定的url，并跳转到该URL。
  - window.location.replace(url)：用指定的URL替换当前窗口的历史记录中的当前条目，并在浏览器的历史记录中创建一个新的记录，使得用户不能使用“后退”按钮返回到前一个页面。
  - window.location.reload()：重新加载当前页面。


# location对象

## location对象的概念和作用

`location`对象是 JavaScript 中一个预定义的全局对象，表示当前页面的 URL 地址和相关信息。它提供了许多有用的属性和方法，用于获取和操作当前页面的 URL 地址。

window对象和location对象是 BOM（浏览器对象模型）中的两个重要对象，它们之间有一定的关系。

可以通过window.location或location的方式来访问和修改当前文档的URL。window.location.href和location.href都可以返回当前文档的完整URL，它们是等价的。window.location.assign()和location.assign()都可以用来加载一个新的URL，并在浏览器的历史记录中添加一个新的记录。

因此，可以说location对象是window对象的一部分(下同)，同时也是window对象中的一个属性，用来表示当前文档的URL。在使用BOM操作时，我们通常需要同时使用window对象和location对象来实现我们的需求。

## location对象的属性和方法

- `location.href`：获取当前页面的完整 URL 地址。
- `location.protocol`：获取当前页面的协议（例如：http 或 https）。
- `location.host`：获取当前页面的主机名（包括端口号）。
- `location.hostname`：获取当前页面的主机名（不包括端口号）。
- `location.port`：获取当前页面的端口号。
- `location.pathname`：获取当前页面的路径部分。
- `location.search`：获取当前页面的查询字符串部分。
- `location.hash`：获取当前页面的锚点部分。
- `location.assign(url)`：用于加载一个新的文档。
- `location.reload()`：用于重新加载当前文档。
- `location.replace(url)`：用于替换当前文档，不会在浏览器历史记录中创建新条目。

# history对象

## history对象的概念和作用

`history` 对象是 JavaScript 中一个预定义的全局对象，表示浏览器的历史记录。它提供了一些有用的方法和属性，用于操作浏览器的历史记录，实现前进、后退等操作。

## history对象的属性和方法

1. history.back()：在浏览器历史记录中后退一页。
2. history.forward()：在浏览器历史记录中前进一页。
3. history.go(n)：在浏览器历史记录中向前或向后跳转n个页面，其中n可以是负数。
4. history.pushState(stateObj, title, url)：将新的URL添加到浏览器历史记录中，但不会导致页面刷新。
5. history.replaceState(stateObj, title, url)：将当前页面的URL替换为新的URL，但不会导致页面刷新。

# navigator对象

## navigator对象的概念和作用

`navigator` 对象是 JavaScript 中一个预定义的全局对象，提供了有关浏览器的信息，例如浏览器的名称、版本、操作系统等信息。通过 `navigator` 对象，我们可以检测当前浏览器的特性，编写出更加兼容不同浏览器的 JavaScript 代码。

## navigator对象的属性和方法

1. `navigator.userAgent`：返回当前浏览器的用户代理字符串，包含浏览器名称、版本、操作系统等信息。
2. `navigator.appName`：返回当前浏览器的名称，例如 "Netscape" 或 "Microsoft Internet Explorer"。
3. `navigator.appVersion`：返回当前浏览器的版本，例如 "5.0 (Windows; en-US)"。
4. `navigator.platform`：返回当前浏览器的操作系统平台，例如 "Win32" 或 "MacIntel"。
5. `navigator.language`：返回当前浏览器的首选语言，例如 "en-US" 或 "zh-CN"。
6. `navigator.geolocation` 方法可以检测当前浏览器是否支持地理位置定位功能。

# screen对象

## screen对象的概念和作用

`screen` 对象是 JavaScript 中一个预定义的全局对象，代表当前浏览器所在的屏幕。`screen` 对象提供了有关当前屏幕的信息，例如屏幕的宽度、高度、可见区域大小等信息。通过 `screen` 对象，我们可以根据不同屏幕的大小和分辨率，动态调整页面布局，以适应不同的设备。

## screen对象的属性和方法

1. `screen.width`：返回屏幕的宽度，单位为像素。
2. `screen.height`：返回屏幕的高度，单位为像素。
3. `screen.availWidth`：返回屏幕可用区域的宽度，单位为像素。
4. `screen.availHeight`：返回屏幕可用区域的高度，单位为像素。
5. `screen.pixelDepth`：返回屏幕的颜色深度，单位为位数。
6. `screen.orientation`：返回屏幕的方向，例如 "landscape-primary" 或 "portrait-secondary"。

# 安全问题

1. 弹出窗口被浏览器屏蔽

   浏览器通常会默认屏蔽弹出窗口。如果网站滥用弹出窗口，可能会被浏览器识别为恶意行为而被屏蔽。

2. 跨域脚本攻击（XSS）

   由于浏览器存在同源策略（Same-Origin Policy），因此在跨域访问时，BOM 操作可能会导致安全问题。

   a. 在一个 iframe 中嵌入一个来自其他域名的页面，如果页面中存在恶意代码，则可能通过 BOM 操作来窃取用户的敏感信息。

   b. 如果我们使用 `location.href` 来修改 URL，而这个 URL 包含恶意脚本，那么这些恶意脚本可能会在用户浏览该页面时执行。

   因此，我们需要确保 URL 是安全的，并对用户输入进行验证和过滤。

3. 误操作

   BOM 操作可能会被用于欺骗用户，例如通过弹出窗口来模拟登录页面，以达到盗取用户密码的目的。

4. 脚本注入

   由于 BOM 操作可以在浏览器中执行任意脚本，因此可能会被用于进行脚本注入攻击，例如将恶意脚本注入到页面中，以达到窃取用户信息或者控制用户计算机的目的。

# 兼容性问题

BOM（浏览器对象模型）是由各个浏览器厂商自行实现的一组 JavaScript API，因此不同的浏览器可能会有一些不同的实现细节，从而导致 BOM 的兼容性问题。

1. 属性和方法的兼容性

   不同浏览器对 BOM 中的属性和方法的实现可能存在差异，例如 IE 和 Firefox 对于 window.innerHeight 和 window.innerWidth 的实现就不同。因此，在编写代码时，需要根据浏览器类型进行兼容性处理。

2. 事件的兼容性

   不同浏览器对事件的处理方式也可能存在差异。例如，在 IE 中，可以使用 window.attachEvent() 来绑定事件，而在其他浏览器中则需要使用 window.addEventListener()。因此，为了确保代码在各种浏览器中都能正常运行，需要根据浏览器类型来选择不同的事件绑定方式。

3. 对象的兼容性

   有些浏览器可能会对 BOM 中的某些对象进行扩展或者修改，从而导致在其他浏览器中无法使用。例如，IE 中提供了 ActiveXObject 对象来访问浏览器的各种功能，而在其他浏览器中则没有这个对象。

为了解决这些兼容性问题，我们可以使用一些兼容性库或者框架，例如 jQuery、Modernizr 等，它们可以屏蔽底层浏览器差异，并提供一致的跨浏览器的 API 接口。此外，在编写代码时，也应该注意遵守一些兼容性的最佳实践，例如避免使用浏览器特定的特性，尽量使用标准的 API 接口等等。

# BOM和DOM

## BOM和DOM的关系和区别

BOM（浏览器对象模型）和DOM（文档对象模型）都是用来描述浏览器中的文档的模型，但是它们的作用不同，有一些区别。

BOM是浏览器提供的一组JavaScript API，用来描述浏览器窗口或标签页以及浏览器本身的状态和功能。BOM包括了window、navigator、location、history等对象，这些对象都是浏览器提供的全局对象，用来描述浏览器的各种状态和行为。BOM主要用来操作浏览器的窗口、页面导航、历史记录、定时器等功能。

DOM则是描述文档的结构、内容和样式的一组API，通过DOM可以访问和操作文档中的元素、属性、文本内容和样式等。DOM主要用来操作HTML、XML等文档，可以用来添加、修改、删除文档的元素和属性，以及操作文档的事件和样式等。

BOM和DOM的关系在于，它们都是JavaScript操作浏览器中的文档的接口，而且它们的对象都是全局对象，可以通过JavaScript代码直接访问和操作。但是它们的作用和目标不同，BOM主要用来操作浏览器的窗口和功能，而DOM主要用来操作文档的结构和内容。DOM和BOM虽然都是浏览器提供的对象模型，但它们并不是标准，不同浏览器之间可能会存在一些差异。

## 如何在BOM和DOM之间进行通信和交互

1. 使用window对象：BOM中的window对象是操作浏览器窗口和页面的入口点，它包含了document、location、history等对象，可以通过window对象来访问和操作这些对象。同时，DOM中的文档对象也可以通过window对象来访问，如window.document、window.frames等。
2. 使用iframe元素：在DOM中，可以使用iframe元素来在文档中嵌入其他文档或网页。在嵌入的文档中，可以通过window.parent对象来访问父文档的window对象，从而实现与父文档的通信和交互。
3. 使用postMessage方法：postMessage是HTML5新增的方法，用于在不同窗口或标签页之间进行跨域通信。可以通过postMessage方法向其他窗口发送消息，并通过事件监听器来接收响应。
4. 使用cookie和localStorage：cookie和localStorage是BOM中提供的两个数据存储方式，可以在不同窗口或标签页之间共享数据。可以在一个窗口中使用cookie或localStorage存储数据，在另一个窗口中通过读取cookie或localStorage来获取数据。

BOM和DOM之间的通信和交互需要注意安全问题，特别是跨域通信时需要考虑安全策略和防止恶意攻击。同时，不同浏览器对BOM和DOM的实现可能存在差异，需要进行兼容性处理。

# 性能优化

BOM 操作的性能问题主要表现在以下两个方面：

1. DOM 操作的阻塞

   BOM 操作通常需要与 DOM 操作配合使用，如果 DOM 操作过于频繁或者复杂，可能会阻塞浏览器的渲染进程，导致页面卡顿或者崩溃。

2. 大量计算的开销

   BOM 操作可能会涉及到大量的计算和操作，例如浏览器窗口大小的计算、浏览器标识的解析等，这些计算和操作可能会对页面的性能产生不良影响。

为了避免 BOM 操作的性能问题，我们可以采取以下几个策略：

1. 减少 DOM 操作的频率和复杂度

   在使用 BOM 操作的时候，尽量减少 DOM 操作的频率和复杂度，避免过多的操作阻塞浏览器的渲染进程。例如，在执行一系列的 DOM 操作之前，可以将这些操作合并成一个批量操作，然后一次性执行。

2. 缓存计算结果

   对于需要频繁计算的操作，可以将计算结果缓存起来，避免重复计算。例如，在获取浏览器窗口大小的时候，可以将结果缓存起来，在窗口大小没有发生变化的情况下，直接使用缓存结果。

3. 避免频繁访问 BOM 属性和方法

   在访问 BOM 属性和方法的时候，尽量避免频繁的访问和调用。例如，在获取浏览器标识的时候，可以将标识缓存起来，避免重复调用。

4. 使用异步操作

   对于需要耗时的操作，可以将操作异步化，避免阻塞页面的渲染进程。例如，在加载大量图片的时候，可以使用异步加载的方式，避免阻塞页面的渲染。

5. 避免使用重绘和重排

   在操作 BOM 时，会触发浏览器的重绘和重排，而这些操作通常是比较耗时的。为了避免这种情况，我们应该尽可能减少重绘和重排的次数。

为了避免 BOM 操作的性能问题，我们需要尽可能地减少与浏览器的交互次数，批量处理数据，避免使用循环，减少重绘和重排的次数，以及减少 DOM 操作的次数。

