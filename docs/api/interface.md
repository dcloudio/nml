## 界面交互
### 分享
#### 接口声明

{"name": "system.share"}

#### uni.share.share(OBJECT)

分享数据到其他app。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|type|String|是|数据的MIME TYPE，要求字母全小写。|
|data|String|是|分享的数据|
|success|Function|否|成功回调。因为大部分 android app 都没有正确的返回分享状态，所以即使分享成功了，也可能执行cancel回调，而不是success回调。|
|fail|Function|否|失败回调|
|cancel|Function|否|取消回调|
|complete|Function|否|执行结束后的回调|

data 的值

- 如果 type 是 text/ 开头的 mimetype（如 text/plain），则 data 是要分享的文本内容
- 如果 type 是其他值，则 data 是要分享的文件路径

支持三种文件路径：
- 通过 uni.fetch.fetch 下载的文件路径
- 通过 uni.file.save 或 list 获得的文件路径
- 以 / 开头的应用内容的资源文件


示例
```javascript
uni.share.share({
  type: "text/html",
  data: "<b>bold</b>",
  success: function (data) { console.log("handling success"); },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 弹窗

#### 接口声明

{"name": "system.prompt"}

#### uni.prompt.showToast(OBJECT)

显示Toast。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|message|String|是|要显示的文本|
|duration|Number|否|0为短时，1为长时，默认0。|

示例
```javascript
uni.prompt.showToast({
  message: 'message'
});
```

#### uni.prompt.showDialog(OBJECT)

显示对话框。

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|title|String|否|标题|
|message|String|否|内容|
|buttons|Array|否|按钮的数组|
|success|Function|否|成功回调|
|cancel|Function|否|取消回调|
|complete|Function|否|执行结束后的回调|

buttons
- 结构 ``{text: 'text', color: '#333333'}``，color 可选。
- 第1项为 positive button
- 第2项（如果有）为 negative button
- 第3项（如果有）为 neutral button
- 最多支持3个 button
```

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|index|Number|选中按钮在 buttons 数组中的序号|

示例
```javascript
uni.prompt.showDialog({
  title: 'title',
  message: 'message',
  buttons: [
    {
      text: 'btn',
      color: '#33dd44'
    }
  ],
  success: function (data) {
    console.log("handling callback");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.prompt.showContextMenu(OBJECT)

显示上下文菜单。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|itemList|Array|是|按钮的文字数组|
|itemColor|HexColor|否|按钮颜色|
|success|Function|否|成功回调|
|cancel|Function|否|取消回调|
|complete|Function|否|执行结束后的回调|

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|index|Number|选中按钮在 itemList 数组中的序号|

示例
```javascript
uni.prompt.showContextMenu({
  itemList: ['item1', 'item2'],
  itemColor: '#ff33ff',
  success: function (data) {
    console.log("handling callback");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 打开网页

#### 接口声明

{"name": "system.webview"}

#### uni.webview.loadUrl(OBJECT)

打开网页，标题栏样式与打开webview的页面的标题栏样式相同。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|url|String|是|要加载的页面url|

示例
```javascript
uni.webview.loadUrl({
  url: 'http://www.example.com'
});
```

#### uni.system.go(path)

WebView 内部 API，在 Webview 打开的网页中可以使用的 API。跳转到当前应用的指定页面。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|path|String|是|要返回的页面。例：``/detail?param1=value1`` 特殊的，如果 path 的值是"/"，则跳转到 path 为"/"的页，没有则跳转到首页。|

示例

```javascript
uni.system.go("/detail?param1=value1");
```

### 通知消息

#### 接口声明

{"name": "system.notification"}

#### uni.notification.show(OBJECT)

显示通知。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|contentTitle|String|否|标题|
|contentText|String|否|内容|
|clickAction|Object|否|通知点击后触发动作的信息|

clickAction 说明

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|uri|String|是|点击通知后跳转的页面地址。|

uri 支持的格式

- 以"/"开头的应用内页面路径。如：``/about``
- 以非"/"开头的应用内页面的名称。如：``About``
- 特殊的，如果uri的值是"/"，则跳转到 path 为"/"的页，没有则跳转到首页。
- 可以通过"?param1=value1"的方式添加参数，参数可以在页面中通过 ``this.param1`` 的方式使用。

示例
```javascript
uni.notification.show({
  contentTitle: 'title',
  clickAction: {
    uri: '/index.html?index=1'
  }
});
```

### 震动

#### 接口声明

{"name": "system.vibrator"}

#### uni.vibrator.vibrate()

触发震动，持续1秒。

参数

无。

示例

```
uni.vibrator.vibrate()
```

### 消息通道 

#### 接口声明

无需声明。

#### uni.channel.subscribe(OBJECT)

监听消息通道，同一个页面对同一个通道多次调用，仅最后一次调用生效。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|name|String|是|通道名称|
|callback|Function|是|每次有新消息，都会被回调。|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|message|String|消息内容|
|source|String|消息来源页面的路径|

示例
```
uni.channel.subscribe({
  name: "channel-1",
  callback: function (data) {
    console.log("handling success: " + data.message);
  }
});
```

#### uni.channel.unsubscribe()

取消监听消息通道。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|name|String|是|通道名称|

示例
```
uni.channel.unsubscribe("channel-1")
```

#### uni.channel.postMessage(OBJECT)

向消息通道发消息。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|name|String|是|通道名称|
|message|String|是|消息内容|


示例
```
uni.channel.postMessage({
  name: "channel-1",
  message: "message-1"
});
```
