# 什么是跨域

要掌握跨域，首先要知道为什么会有跨域这个问题的出现。那要知道为什么有跨域，就离不开同源策略这个概念了。

## 同源策略

同源策略（_Sameoriginpolicy_）是浏览器端的一个安全策略。_1995_ 年，同源政策由 _Netscape_ 公司引入浏览器。目前，所有浏览器都实行这个政策。

### 含义

同源策略的含义是指，_A_ 网页加载的文档或脚本不能与 _B_ 网页的资源进行交互，除非这两个网页"同源"。

所谓"同源"指的是"三个相同"：

- 协议相同
- 域名相同
- 端口相同

例如，`http://www.test.com/` 这个网址：

- 协议是 `http://`
- 域名是 `www.test.com`
- 端口是 _80_（_http_ 的默认端口可以省略）

它的同源情况如下：

| 当前页面 URL                                       | 被请求页面 URL                                                     | 是否跨域 | 原因                           |
| :------------------------------------------------- | :----------------------------------------------------------------- | :------- | :----------------------------- |
| [http://www.test.com/](http://www.test.com/)       | [http://www.test.com/index.html](http://www.test.com/index.html)   | 否       | 同源（协议、域名、端口号相同） |
| [http://www.test.com/](http://www.test.com/)       | [https://www.test.com/index.html](https://www.test.com/index.html) | 跨域     | 协议不同（_http_ / _https_）   |
| [http://www.test.com/](http://www.test.com/)       | [http://www.baidu.com/](http://www.baidu.com/)                     | 跨域     | 主域名不同（_test_ / _baidu_） |
| [http://www.test.com/](http://www.test.com/)       | [http://blog.test.com/](http://blog.test.com/)                     | 跨域     | 子域名不同（_www_ / _blog_）   |
| [http://www.test.com:80/](http://www.test.com:80/) | [http://www.test.com:7001/](http://www.test.com:7001/)             | 跨域     | 端口号不同（_80_ / _7001_）    |

### 目的

同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。

设想这样一种情况：

你正在浏览器中使用网上银行进行转账汇款等操作，这个时候朋友发送了一个“只可意会，不可言传”的链接给你，你怀着一颗虔诚的心点开了它，谁知道，这个网站在暗地里做了些不可描述的事情！

如果这个网站可以读取银行网站的 _Cookie_，会发生什么？

很显然，当 _Cookie_ 中包含一些隐私数据（比如存款总额），这些信息就会泄漏。更可怕的是，_Cookie_ 往往用来保存用户的登录状态，如果用户没有退出登录，其他网站就可以冒充用户，为所欲为。因为浏览器同时还规定，提交表单不受同源政策的限制。

由此可见，"同源政策"是必需的，否则 _Cookie_ 可以共享，互联网就毫无安全可言了。

### 限制范围

随着互联网的发展，"同源政策"越来越严格。目前，如果在非同源的情况下，共有三种行为受到限制：

1. 无法读取非同源网页的 _Cookie、LocalStorage_ 和 _IndexedDB_；
2. 无法接触非同源网页的 _DOM_；
3. 无法向非同源地址发送 _AJAX_ 请求。

## 跨域的定义

当一个 _URL_ 请求的**协议、域名、端口**三者中任意一个与当前页面 _URL_ 不同，即为跨域。跨域问题的出现正是由于「浏览器的同源策略」限制。

**注：_localhost_ 和 _127.0.0.1_ 虽然都指向本机，但也属于跨域。**

## 跨域的解决方案

### _Cookie_ 跨域的解决方案

_Cookie_ 是服务器写入浏览器的一小段信息，只有同源的网页才能共享。而浏览器是通过 `document.domain` 属性来检查两个页面是否同源，因此，当两个网页一级域名相同，只是二级域名不同时，浏览器允许通过设置 `document.domain` 共享 _Cookie_。

举个栗子：

_A_ 网页是 `http://w1.example.com/a.html`，_B_ 网页是 `http://w2.example.com/b.html`，那么只要给两个页面都设置相同的 `document.domain`，两个网页就可以共享 _Cookie_。

```js
// 两个页面都设置
document.domain = "example.com";
```

现在，_A_ 网页通过脚本设置一个 _Cookie_。

```js
document.cookie = "test1=hello";
```

_B_ 网页就可以读到这个 _Cookie_。

```js
var allCookie = document.cookie;
```

> 注意，该方法只适用于 _Cookie_ 和 _iframe_ 窗口，_LocalStorage_ 和 _IndexDB_ 无法通过这种方法规避同源政策。

另外，服务器也可以在设置 _Cookie_ 的时候，指定 _Cookie_ 的所属域名为一级域名，比如 `.example.com`。

```js
Set-Cookie: key=value; domain=.example.com; path=/
```

这样的话，二级域名和三级域名不用做任何设置，都可以读取这个 _Cookie_。

### _DOM_ 操作跨域的解决方案

如果两个网页不同源，就无法拿到对方的 _DOM_。典型的例子是 `iframe` 窗口和 `window.open` 方法打开的窗口，它们与父窗口无法通信。

举个栗子：

父窗口运行下面的命令，如果 `iframe` 窗口不是同源，就会报错。

```js
document.getElementById("myIFrame").contentWindow.document;
// Uncaught DOMException: Blocked a frame from accessing a cross-origin frame.
```

上面命令中，父窗口想获取子窗口的 _DOM_，因为跨源导致报错。

反之亦然，子窗口获取主窗口的 _DOM_ 也会报错。

```js
window.parent.document.body;
// 报错
```

如果两个窗口一级域名相同，只是二级域名不同，那么设置上一节介绍的 `document.domain` 属性，就可以规避同源政策，拿到 _DOM_。

对于完全不同源的网站，目前有三种方法，可以解决跨域窗口的通信问题：

- 片段识别符（_fragment identifier_）
- _window.name_
- 跨文档通信 _API_（_Cross-document messaging_）

#### 1）片段识别符

片段标识符（_fragment identifier_）指的是，_URL_ 的 `#` 号后面的部分，比如 `http://example.com/x.html#fragment` 的 `#fragment`。如果只是改变片段标识符，页面不会重新刷新。

父窗口可以把信息，写入子窗口的片段标识符。

```js
var src = originURL + "#" + data;
document.getElementById("myIFrame").src = src;
```

子窗口通过监听 `hashchange` 事件得到通知。

```js
window.onhashchange = checkMessage;

function checkMessage() {
  var message = window.location.hash;
  // ...
}
```

同样的，子窗口也可以改变父窗口的片段标识符。

```js
parent.location.href = target + "#" + hash;
```

#### 2）_window.name_

浏览器窗口有 `window.name` 属性。这个属性的最大特点是，无论是否同源，只要在同一个窗口里，前一个网页设置了这个属性，后一个网页可以读取它。

父窗口先打开一个子窗口，载入一个不同源的网页，该网页将信息写入 `window.name` 属性。

```js
window.name = data;
```

接着，子窗口跳回一个与主窗口同域的网址。

```js
location = "http://parent.url.com/xxx.html";
```

然后，主窗口就可以读取子窗口的 `window.name` 了。

```js
var data = document.getElementById("myFrame").contentWindow.name;
```

这种方法的优点是，`window.name` 容量很大，可以放置非常长的字符串；缺点是必须监听子窗口 `window.name` 属性的变化，影响网页性能。

#### 3）跨文档通信 _API_

上面两种方法都属于破解，_HTML5_ 为了解决这个问题，引入了一个全新的 _API_：跨文档通信 _API_（_Cross-document messaging_）。

这个 _API_ 为 `window` 对象新增了一个 `window.postMessage` 方法，允许跨窗口通信，不论这两个窗口是否同源。

举个栗子：

父窗口 `http://aaa.com` 向子窗口 `http://bbb.com` 发消息，调用 `postMessage` 方法就可以了。

```js
var popup = window.open("http://bbb.com", "title");
popup.postMessage("Hello World!", "http://bbb.com");
```

`postMessage` 方法的第一个参数是具体的信息内容，第二个参数是接收消息的窗口的源（_origin_），即"协议 + 域名 + 端口"。也可以设为 `*`，表示不限制域名，向所有窗口发送。

子窗口向父窗口发送消息的写法类似。

```js
window.opener.postMessage("Nice to see you", "http://aaa.com");
```

父窗口和子窗口都可以通过 `message` 事件，监听对方的消息。

```js
window.addEventListener(
  "message",
  function (e) {
    console.log(e.data);
  },
  false
);
```

`message` 事件的事件对象 `event`，提供以下三个属性：

- `event.source`：发送消息的窗口
- `event.origin`: 消息发向的网址
- `event.data`: 消息内容

下面的例子是，子窗口通过 `event.source` 属性引用父窗口，然后发送消息。

```js
window.addEventListener("message", receiveMessage);
function receiveMessage(event) {
  event.source.postMessage("Nice to see you!", "*");
}
```

`event.origin` 属性可以过滤不是发给本窗口的消息。

```js
window.addEventListener("message", receiveMessage);
function receiveMessage(event) {
  if (event.origin !== "http://aaa.com") return;
  if (event.data === "Hello World") {
    event.source.postMessage("Hello", event.origin);
  } else {
    console.log(event.data);
  }
}
```

### _localStorage_ 跨域的解决方案

通过 `window.postMessage`，读写其他窗口的 _localStorage_ 也成为了可能。

下面是一个例子，主窗口写入 _iframe_ 子窗口的 `localStorage`。

```js
window.onmessage = function (e) {
  if (e.origin !== "http://bbb.com") {
    return;
  }
  var payload = JSON.parse(e.data);
  localStorage.setItem(payload.key, JSON.stringify(payload.data));
};
```

上面代码中，子窗口将父窗口发来的消息，写入自己的 _localStorage_。

父窗口发送消息的代码如下：

```js
var win = document.getElementsByTagName("iframe")[0].contentWindow;
var obj = { name: "Jack" };
win.postMessage(
  JSON.stringify({ key: "storage", data: obj }),
  "http://bbb.com"
);
```

加强版的子窗口接收消息的代码如下：

```js
window.onmessage = function (e) {
  if (e.origin !== "http://bbb.com") return;
  var payload = JSON.parse(e.data);
  switch (payload.method) {
    case "set":
      localStorage.setItem(payload.key, JSON.stringify(payload.data));
      break;
    case "get":
      var parent = window.parent;
      var data = localStorage.getItem(payload.key);
      parent.postMessage(data, "http://aaa.com");
      break;
    case "remove":
      localStorage.removeItem(payload.key);
      break;
  }
};
```

加强版的父窗口发送消息代码如下：

```js
var win = document.getElementsByTagName("iframe")[0].contentWindow;
var obj = { name: "Jack" };
// 存入对象
win.postMessage(
  JSON.stringify({ key: "storage", method: "set", data: obj }),
  "http://bbb.com"
);
// 读取对象
win.postMessage(JSON.stringify({ key: "storage", method: "get" }), "*");
window.onmessage = function (e) {
  if (e.origin != "http://aaa.com") return;
  // "Jack"
  console.log(JSON.parse(e.data).name);
};
```

### _AJAX_ 跨域的解决方案

同源政策规定，_AJAX_ 请求只能发给同源的网址，否则就报错。

除了架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务）外，还有三种方法规避这个限制：

- _JSONP_
- _WebSocket_
- _CORS_

#### _JSONP_

_JSONP_ 是服务器与客户端跨源通信的常用方法。最大特点就是简单适用，老式浏览器全部支持，服务器改造非常小。但有一个缺点是只支持 `GET` 的请求类型。

它的基本思想是，利用 `<script>` 标签天生支持跨域的特点，在页面中添加一个 `<script>` 元素，向服务器请求 _JSON_ 数据，服务器收到请求后，将数据放在一个指定名字的回调函数的参数位置传回来。

**原生 _JS_**

首先，网页动态插入 `<script>` 元素，由它向跨源网址发出请求。

```html
<!-- 处理服务器返回回调函数的数据 -->
<script type="text/javascript">
  function dosomething(res) {
    // 处理获得的数据
    console.log(res.data);
  }
</script>

<!-- 向服务器发出请求，该请求的查询字符串有一个callback参数，用来指定回调函数的名字 -->
<script src="http://localhost:8080/students?callback=dosomething"></script>
```

上面代码通过 `<script>` 元素，向服务器发出请求。该请求的查询字符串有一个 `callback` 参数，用来指定回调函数的名字，这对于 _JSONP_ 是必需的。

服务器收到这个请求处理完成后，会将数据放在回调函数的参数位置返回。

```js
res.send(`${dosomething({ data: "hello" })}`);
```

由于 `<script>` 元素请求的脚本，直接作为代码运行。这时，只要浏览器定义了 `dosomething` 函数，该函数就会立即调用。作为参数的 _JSON_ 数据被视为 _JavaScript_ 对象，而不是字符串，因此避免了使用 `JSON.parse` 的步骤。

**_jQuery_ 的 _AJAX_**

```js
$.ajax({
  url: "http://www.test.com:8080/login",
  type: "get",
  dataType: "jsonp", // 请求方式为 jsonp
  data: {},
  success(msg) {},
});
```

服务器收到请求处理完成后，会通过 jsonp 的方法将数据返回。

```js
res.jsonp(data);
```

#### _WebSocket_

_WebSocket_ 是一种通信协议，使用 `ws://`（非加密）和 `wss://`（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。

下面是一个例子，浏览器发出的 _WebSocket_ 请求的头信息：

```plain
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```

上面代码中，有一个字段是 `Origin`，表示该请求的请求源（_origin_），即发自哪个域名。

正是因为有了 `Origin` 这个字段，所以 _WebSocket_ 才没有实行同源政策。因为服务器可以根据这个字段，判断是否许可本次通信。如果该域名在白名单内，服务器就会做出如下回应。

```plain
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```

#### CORS

_CORS_ 是跨源资源分享（Cross-Origin Resource Sharing）的缩写。它是 _W3C_ 标准，是跨源 _AJAX_ 请求的根本解决方法。相比 _JSONP_ 只能发送 `GET` 请求，_CORS_ 允许任何类型的请求。

_CORS_ 处理跨域请求分为以下两种情况：

1. 普通跨域请求：只需服务器端设置 _Access-Control-Allow-Origin_。
2. 带 _Cookie_ 的跨域请求：前后端都需要进行设置

##### 前端设置

先根据 `xhr.withCredentials` 字段设置是否携带 _cookie_。

1. 原生 _ajax_

   ```js
   var xhr = new XMLHttpRequest(); // IE8/9需用window.XDomainRequest兼容

   // 前端设置是否带 cookie
   xhr.withCredentials = true;

   xhr.open("post", "http://www.domain2.com:8080/login", true);
   xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
   xhr.send("user=admin");

   xhr.onreadystatechange = function () {
     if (xhr.readyState == 4 && xhr.status == 200) {
       alert(xhr.responseText);
     }
   };
   ```

2. _jQuery ajax_

   ```js
   $.ajax({
     url: "http://www.test.com:8080/login",
     type: "get",
     data: {},
     xhrFields: {
       withCredentials: true, // 前端设置是否带 cookie
     },
     crossDomain: true, // 会让请求头中包含跨域的额外信息，但不会含cookie
   });
   ```

3. _vue-resource_

   ```js
   Vue.http.options.credentials = true;
   ```

4. _axios_

   ```javascript
   axios.defaults.withCredentials = true;
   ```

##### 服务端设置

服务器端对于 _CORS_ 的支持，主要是通过设置 _Access-Control-Allow-Origin_ 来进行的。如果浏览器检测到相应的设置，就可以允许 _AJAX_ 进行跨域的访问。

1. _Node.js_ 后台

   ```js
   var http = require("http");
   var server = http.createServer();
   var qs = require("querystring");

   server.on("request", function (req, res) {
     var postData = "";

     // 数据块接收中
     req.addListener("data", function (chunk) {
       postData += chunk;
     });

     // 数据接收完毕
     req.addListener("end", function () {
       postData = qs.parse(postData);

       // 跨域后台设置
       res.writeHead(200, {
         // 后端允许发送Cookie
         "Access-Control-Allow-Credentials": "true",
         // 允许访问的域（协议+域名+端口）
         "Access-Control-Allow-Origin": "http://www.domain1.com",
         /*
          * 此处设置的cookie还是domain2的而非domain1，因为后端也不能跨域写cookie(nginx反向代理可以实现)，
          * 但只要domain2中写入一次cookie认证，后面的跨域接口都能从domain2中获取cookie，从而实现所有的接口都能跨域访问
          */
         // HttpOnly的作用是让js无法读取cookie
         "Set-Cookie": "l=a123456;Path=/;Domain=www.domain2.com;HttpOnly",
       });
       res.write(JSON.stringify(postData));
       res.end();
     });
   });

   server.listen("8080");
   console.log("Server is running at port 8080...");
   ```

2. _Node.js_ 的 _Express_ 后台

   ```js
   var allowCrossDomain = function (req, res, next) {
     // 设置允许跨域访问的请求源（* 表示接受任意域名的请求）
     res.header("Access-Control-Allow-Origin", "*");
     // 设置允许跨域访问的请求头
     res.header(
       "Access-Control-Allow-Headers",
       "X-Requested-With,Origin,Content-Type,Accept"
     );
     // 设置允许跨域访问的请求类型
     res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");
     // 同意 cookie 发送到服务器（如果要发送cookie，Access-Control-Allow-Origin 不能设置为星号）
     res.header("Access-Control-Allow-Credentials", "true");
   };
   app.use(allowCrossDomain);
   ```
