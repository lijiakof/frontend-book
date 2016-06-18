# HTML5 新特性之 Websocket
HTML5 为 Web 应用带来了一种新的通信方式，它不同于 Ajax 通信方式在于 Websocket 是一种客户端与服务器端可以双工通信，这样一来客户端不仅可以拉取服务器端的数据，而且服务器端可以主动的向客户的推送数据。这样一来在 Web 应用中可以实现更多的数据交互方式，完成更佳好的数据体验。

在 Websocket 出现之前，网站为了实现这种即时通讯，都是采用轮询拉取服务端的数据，虽然这种方式可以完成类似于数据推送的通信，但是在即时性和性能上都不如 Websocket 技术。

## Websocket API
### 创建一个实例
创建一个 WebSocket 实例，即和 WebSocket 服务器之间建立了连接：

```
var socket = new WebSocket('ws://domain:port'); 
```

### send(msg)
它是用来给 WebSocket 服务器发送数据的方法：

```
var socket = new WebSocket('ws://domain:port'); 
socket.send("send message to server");
```

### close()
它是用来关闭和 WebSocket 服务器之间的连接：

```
var socket = new WebSocket('ws://domain:port'); 
socket.close();
```

### onopen
它是用来监听客户端链接 WebSocket 服务器成功的事件：

```
var socket = new WebSocket('ws://domain:port'); 
socket.onopen = function(evt){
};
```

### onerror
它是用来监听客户端和 WebSocket 服务器数据交互产生错误（连接失败、发送或者接收数据失败、处理事件失败等）的事件：

```
var socket = new WebSocket('ws://domain:port'); 
socket.onerror = function(evt){
};
```

### onmessage
它是用来监听客户端和 WebSocket 服务器之间发送数据的事件，其中数据会包含在回调函数的传入参数`evt`中：

```
var socket = new WebSocket('ws://domain:port'); 
socket.onmessage = function(evt){
};
```

### onclose
它是用来监听客户端与 WebSocket 服务器断开连接的事件：

```
var socket = new WebSocket('ws://domain:port'); 
socket.onclose = function(evt){
};
```

## 总结
这种长链接的双工通信方式让 Web 应用在体验上又更佳提升了一部，并且在性能上面有了一定的提升，它让 Web 的数据更佳实时的展现出来，并且它让 Web 应用拓展到即时通讯、游戏等行业，让 Web 的应用性更佳的宽广。

## 参考
* http://www.ibm.com/developerworks/cn/java/j-lo-WebSocket/

