### 推送 

#### 接口声明

{"name": "service.push"}

#### uni.push.getProvider(OBJECT) 

获取服务提供商。

参数

无。

返回值

字符串，服务提供商的代号，如厂商的英文品牌名称，假如无此服务则返回空字符串。

示例
```javascript
uni.push.getProvider();
```

#### uni.push.subscribe(OBJECT)

订阅push，后续可以收到push消息（一般可在应用初始化的地方进行调用。比如在app的onCreate方法中调用）。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调，返回 regId,用于发送消息。|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.push.subscribe({
  success: function(data) {
    console.log("push.subscribe succeeded, result data=" + JSON.stringify(data));
  },
  fail: function(data, code) {
    console.log("push.subscribe failed, result data=" + JSON.stringify(data) + ", code=" + code);
  },
  complete: function() {
    console.log("push.subscribe completed");
  }
});
```

#### uni.push.unsubscribe(OBJECT)

取消订阅（一般不建议调用，调用后 regId 失效，需要重新订阅获取新的 regId）。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.push.unsubscribe({
  success: function(data) {
    console.log("push.unsubscribe succeeded, result data=" + JSON.stringify(data));
  },
  fail: function(data, code) {
    console.log("push.unsubscribe failed, result data=" + JSON.stringify(data) + ", code=" + code);
  },
  complete: function() {
    console.log("push.unsubscribe completed");
  }
});
```

#### uni.push.on(OBJECT)

添加push事件回调（透传消息的payload内容可在此回调中收到）。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|callback|Function|是|push事件回调处理|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|messageId|String|消息id|
|data|String|消息内容payload|

示例
```javascript
uni.push.on({
  callback: function(ret) {
    console.log("received pass through message, ret=" + JSON.stringify(ret));
  }
});
```

#### uni.push.off(OBJECT)

移除push事件回调，push.on中的callback不会再收到透传内容。

参数

无。

示例
```javascript
uni.push.off();
```