## 开发与调试
### 使用日志输出
项目根目录下的 manifest.json 文件中，config->logLevel 默认是 debug，即：允许所有级别的日志输出。
```
"config": {
  "logLevel": "debug"/*打印日志等级*/
},
```
与传统前端开发一样，使用 console 输出日志。
```
console.debug('debug');
console.log('log');
console.info('info');
console.warn('warn');
console.error('error');
```
注：目前只能通过**远程调试**查看输出的日志。
### 远程调试
远程调试是在真机运行的同时，借助远程调试工具，来调试手机设备上的应用。

- 菜单->运行->Debug调试->设备
- 调试服务启动成功后，会自动打开 Chrome 浏览器。

#### Debugger
用来调试 UniApp 中的 JS 前端代码。

- “Console”控制台显示前端的 log 信息，并且能根据级别（info/warn/error 等）及关键字过滤。
- “Sources”中能够显示所有 JS 源码，包括 js-framework 等代码。可以设置断点、查看调用栈，和使用 Chrome 浏览器调试操作一样。

### Inspector
将 ElementMode 切换到 vdom，并且点击 Inspector，可以查看 native DOM 树，及其 style 属性和 layout 图。鼠标在 DOM 树移动时，在 device（或模拟器）上对应节点会高亮显示，有助于开发者定位和发现节点。