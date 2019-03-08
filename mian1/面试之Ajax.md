## 1、XMLHttpRequest对象

```js
    function createXHR(){
        if (typeof XMLHttpRequest != "undefined"){
            return new XMLHttpRequest();
        } else if (typeof ActiveXObject != "undefined"){
           if (typeof arguments.callee.activeXString != "string"){
               var versions = [ "MSXML2.XMLHttp.6.0", "MSXML2.XMLHttp.3.0",
               					"MSXML2.XMLHttp"],
 					i, len;

          		for (i=0,len=versions.length; i < len; i++){
                   try {
                       new ActiveXObject(versions[i]);
                       arguments.callee.activeXString = versions[i];
                       break;
                    } catch (ex){
                    //跳过 
                    }
                 } 
			}
       		return new ActiveXObject(arguments.callee.activeXString);
        } else {
            throw new Error("No XHR object available.");
        }
	}
```



## 2、怎样使用xhr？

```
var xhr = createXHR();
```

要调用的第一个方法是 **open()**，它接受 3 个参数:要发送的请求的类型("get"、"post"等)、请求的 URL 和表示是否异步发送请求的布尔值。

```
xhr.open("get", "example.php", false);
```

 **send()**方法接收一个参数，即要作为请求主体发送的数据。如果不需要通过请求主体发送 数据，则必须传入 null，因为这个参数对有些浏览器来说是必需的。

```
xhr.send(null);
```

只要 readyState 属性的值由一个值变成另一个值，都会触发一次 **readystatechange** 事件。

readyState属性可取的值：

```
0:未初始化。尚未调用 open()方法。
1:启动。已经调用 open()方法，但尚未调用 send()方法。
2:发送。已经调用 send()方法，但尚未接收到响应。
3:接收。已经接收到部分响应数据。
4:完成。已经接收到全部响应数据，而且已经可以在客户端使用了。
```

完成的xmr例子：

```js
var xhr = createXHR();
xhr.onreadystatechange = function(){
    if (xhr.readyState == 4){
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
            alert(xhr.responseText);
   		} else {
         	alert("Request was unsuccessful: " + xhr.status);
    	}
	} 
};
xhr.open("get", "example.txt", true);
xhr.send(null);
```

在接收到响应之前还可以调用 **abort()**方法来取消异步请求。调用这个方法后，XHR 对象会停止触发事件，而且也不再允许访问任何与响应有关的对象属性。

## 3、HTTP头部信息

每个 HTTP 请求和响应都会带有相应的头部信息。默认情况下，在发送 XHR 请求的同时，还会发送下列头部信息。

```
Accept:浏览器能够处理的内容类型。
Accept-Charset:浏览器能够显示的字符集。
Accept-Encoding:浏览器能够处理的压缩编码。
Accept-Language:浏览器当前设置的语言。
Connection:浏览器与服务器之间连接的类型。
Cookie:当前页面设置的任何 Cookie。
Host:发出请求的页面所在的域 。
Referer:发出请求的页面的 URI。注意，HTTP 规范将这个头部字段拼写错了，而为保证与规范一致，也只能将错就错了。(这个英文单词的正确拼法应该是 referrer。)
User-Agent:浏览器的用户代理字符串。
```

使用 **setRequestHeader()**方法可以设置自定义的请求头部信息。这个方法接受两个参数:头部字段 的名称和头部字段的值。要成功发送请求头部信息，必须在调用 open()方法之后且调用 send()方法 之前调用setRequestHeader()。

```
xhr.open("get", "example.php", true); 
xhr.setRequestHeader("MyHeader", "MyValue"); 
xhr.send(null);
```

调用 XHR 对象的 **getResponseHeader()**方法并传入头部字段名称，可以取得相应的响应头部信 息。而调用 **getAllResponseHeaders()**方法则可以取得一个包含所有头部信息的长字符串。

## 4、FormData

FormData 类型为序列化表单以及创建与表单格式相同的数据(用于通过 XHR 传输)提供 了便利。

```
var data = new FormData();
data.append("name", "Nicholas");
```

**append()**方法接收两个参数:键和值，分别对应表单字段的名字和字段中包含的值。

```
var form = document.getElementById("user-info");
xhr.send(new FormData(form));
```

使用 FormData 的方便之处体现在**不必明确地在 XHR 对象上设置请求头部**。XHR 对象能够识别传 入的数据类型是 FormData 的实例，并配置适当的头部信息

## 5、超时设定

IE8 为 XHR 对象添加了一个 **timeout** 属性，表示请求在等待响应多少毫秒之后就终止。在给 timeout 设置一个数值后，如果在规定的时间内浏览器还没有接收到响应，那么就会触发 timeout 事 件，进而会调用 **ontimeout** 事件处理程序。

```js
var xhr = createXHR();
xhr.onreadystatechange = function(){
  	if (xhr.readyState == 4){ 
  		try {
            if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
                alert(xhr.responseText);
            } else {
                alert("Request was unsuccessful: " + xhr.status);
            }
        } catch (ex){
        //假设由 ontimeout 事件处理程序处理 
        }
	} 
};

xhr.open("get", "timeout.php", true); 10 
xhr.timeout = 1000; //将超时设置为1秒钟(仅适用于IE8+)
xhr.ontimeout = function(){
    alert("Request did not return in a second.");
};
xhr.send(null);
```

## 6、进度事件

```
loadstart:在接收到响应数据的第一个字节时触发。 
progress:在接收响应期间持续不断地触发。
error:在请求发生错误时触发。
abort:在因为调用 abort()方法而终止连接时触发。 
load:在接收到完整的响应数据时触发。
loadend:在通信完成或者触发 error、abort 或 load 事件后触发。
```

## 7、跨域

**1、CORS**

CORS(Cross-Origin Resource Sharing，跨源资源共享)是 W3C 的一个工作草案，定义了在必须访 问跨源资源时，浏览器与服务器应该如何沟通。CORS 背后的基本思想，就是使用自定义的 HTTP 头部 让浏览器与服务器进行沟通，从而决定请求或响应是应该成功，还是应该失败。

**2、图像Ping**

使用<img>标签。一个网页可以从任何网页中加载图像，不 用担心跨域不跨域。这也是在线广告跟踪浏览量的主要方式。

动态创建图像经常用于**图像 Ping**。图像 Ping 是与服务器进行简单、**单向**的跨域通信的一种方式。 请求的数据是通过查询字符串形式发送的，而响应可以是任意内容，但通常是像素图或 204 响应。通过 图像 Ping，浏览器得不到任何具体的数据，但通过侦听 load 和 error 事件，它能知道响应是什么时 候接收到的。

```
img.onload = img.onerror = function(){
    alert("Done!");
};
img.src = "http://www.example.com/test?name=Nicholas";
```

**3、JSONP**

JSONP 是 JSON with padding(填充式 JSON 或参数式 JSON)的简写，是应用 JSON 的一种新方法。JSONP 由两部分组成:**回调函数**和**数据**。**回调函数是当响应到来时应该在页面中调用的函数**。回调 函数的名字一般是在请求中指定的。而**数据就是传入回调函数中的 JSON 数据**。

```
http://freegeoip.net/json/?callback=handleResponse
```

JSONP 是通过动态<script>元素来使用的，使用时可以为src 属性指定一个跨域 URL。这里的<script>元素与<img>元素类似，都有能力不受限制地从其他域 加载资源。因为 JSONP 是有效的 JavaScript 代码，所以在请求完成后，即在 JSONP 响应加载到页面中 以后，就会立即执行。

**4、Web Sockets**

Web Sockets 的目标是在一个单独的 持久连接上提供全**双工、双向**通信。在 JavaScript 中创建了 Web Socket 之后，会有一个 HTTP 请求发送 到浏览器以发起连接。在取得服务器响应后，建立的连接会使用 HTTP 升级从 HTTP 协议交换为 Web Socket 协议。也就是说，使用标准的 HTTP 服务器无法实现 Web Sockets，只有支持这种协议的专门服 务器才能正常工作。

要**创建 Web Socket**，先实例一个 WebSocket 对象并传入要连接的 URL:

```
var socket = new WebSocket("ws://www.example.com/server.php");
```

必须给 WebSocket 构造函数传入绝对 URL。同源策略对 Web Sockets 不适用，因此可以通 过它打开到**任何站点**的连接。

WebSocket 也有一 个表示当前状态的 readyState 属性。不过，这个属性的值与 XHR 并不相同，

```
WebSocket.OPENING (0):正在建立连接。 
WebSocket.OPEN (1):已经建立连接。 
WebSocket.CLOSING (2):正在关闭连接。 
WebSocket.CLOSE (3):已经关闭连接。
```

Web Socket 打开之后，就可以通过**连接发送和接收数据**。要向服务器发送数据，使用 **send()**方法并传入任意字符串

```
var socket = new WebSocket("ws://www.example.com/server.php");
socket.send("Hello world!");
```

Web Sockets 只能通过连接发送纯文本数据，所以对于复杂的数据结构，在通过连接发送之前， 必须进行序列化。

当服务器向客户端发来消息时，WebSocket 对象就会触发 **message 事件**

```
socket.onmessage = function(event){
    var data = event.data;
	//处理数据 
};
```

要**关闭 Web Socket** 连接，可以在任何时候调用 close()方法。 

```
socket.close();
```

WebSocket 对象还有其他三个事件，在连接生命周期的不同阶段触发。

```
open:在成功建立连接时触发。
error:在发生错误时触发，连接不能持续。
close:在连接关闭时触发。
```















