# serve-socket
一个提供socket服务的工具
#### 项目托管地址
https://github.com/betterzfz/socket-service
#### 环境要求
nodejs
#### 部署
* 创建一个文件夹并把项目文件放进去。
* 配置`./config/default.json`文件, 设置`hostname`和`port`的值，如下：
```json
{
    "hostname" : "127.0.0.1",
    "port" : 3000
}
```
* 在当前目录下执行`npm install`命令来安装项目依赖的`node package`。
* 执行命令`node index.js`来开启服务。

#### 使用
* 在需要`socket`服务的页面包含两个`javascript`文件，如下：
```html
<script src="http://127.0.0.1:3000/socket.io/socket.io.js"></script>
<script src="http://127.0.0.1:3000/socket-service.js"></script>
```
请将文件路径中的`hostname`和`port`设置成与`./config/default.json`中参数相同的值。

* 创建一个可以提供socket服务的对象，如下：
```javascript
const socket_service = new SocketService('127.0.0.1', '3000');
```
* 注册一个事件，如下：
```javascript
socket_service.register('purchase', { id : 1, message : 'hello'});
```
其中`purchase`是事件名称，`{ id : 1, message : 'hello'}`是该事件发送的数据。

* 应用事件，如下:
```javascript
socket_service.apply('purchase', data => {
    console.log(data);
});
```
其中`purchase`是上面注册的事件名称， `data`是接收到的参数。