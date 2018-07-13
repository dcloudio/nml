## 开发与调试
在阅读此部分内容之前，请先完成“入门->HelloWorld”部分关于真机调试的内容。
### 使用日志输出
#### 修改日志等级
项目根目录下的 manifest.json 文件中，config->logLevel 默认是 debug，即：允许所有级别的日志输出。
```
"config": {
  "logLevel": "debug"/*打印日志等级*/
},
```
#### 在 JS 中输出日志
输出日志能帮助开发者快速定位问题，与传统前端开发一样，使用 console 输出日志。
```
console.debug('debug');
console.log('log');
console.info('info');
console.warn('warn');
console.error('error');
```
#### 查看日志
通过**远程调试**查看输出的日志。

### 远程调试
远程调试指的是通过 HBuilderX 的工具调试手机 App 端的页面。

- 菜单->运行->Debug调试->设备
- 调试服务启动成功后，会自动打开 Chrome 浏览器。
- 当手机设备上的应用启动成功后，可以在调试工具界面看到应用信息。
- 按照调试工具界面的说明，就可以进行 JS 断点，查看日志，页面布局等调试工作。