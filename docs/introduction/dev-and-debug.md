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
- 点击调试界面的 inspect 开始调试。