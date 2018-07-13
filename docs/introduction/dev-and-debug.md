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

### Debugger
> Debugger 功能可以对 nml 页面中的 js 进行调试。

#### 断点
找到目标页面的 JS，然后就可以像开发网页一样进行断点调试。

以 hello-nml 的 Home 页为例：Sources panel -> Network -> Runtime.js -> 192.xxx:port -> source -> file:/ -> Home 就是最终编译后的页面 JS。

找到 showDetail 方法添加一个断点，在手机设备上进行触发此方法的操作，然后就可以看到断点生效，并进行后续的调试。

![这里放个图](https://img-cdn-qiniu.dcloud.net.cn/uploads/images/debugger.png)

#### 日志
在 Console panel 可以看到 JS 中输出的日志信息。

### Inspector
> Inspector功能可查看页面的 VDOM/Native Tree 结构

ElmentMode 切换为 vdom，点击 Inspector 打开页面结构的窗口。

左侧为效果预览区域，右侧为开发者工具。通过开发者工具左上角的选择器，可以快速定位页面中的组件对应的页面结构及 nss 属性。
![这里放个图](https://img-cdn-qiniu.dcloud.net.cn/uploads/images/vdom.png)

- virtual 表示实际计算后的结果
- local 表示原始设置的值

ElmentMode 默认为 native，可以查看原生的页面结构。这个功能只是用作预览，没有其它更多的作用。
