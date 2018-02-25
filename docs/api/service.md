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

### 帐号 

#### 接口声明

{"name": "service.account"}

#### uni.account.getProvider(OBJECT)

获取服务提供商，如厂商的英文品牌名称，假如无此服务则返回空字符串。

参数

无。

返回值

字符串，服务提供商的代号。

示例
```javascript
uni.account.getProvider();
```

#### uni.account.authorize(OBJECT)

进行OAuth授权。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|type|String|是|授权码模式为 code，简化模式为 token。|
|redirectUri|Uri|否|重定向 URI。|
|scope|String|否|申请的权限范围，目前只支持一种scope，假如不填则getProfile只返回openId。scope.baseProfile：获取用户基本信息。|
|state|String|否|可以指定任意值，认证服务器会原封不动地返回这个值。|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|state|String|请求时同字段指定的任意值。|
|code|String|授权码模式下可用，返回的授权码。|
|accessToken|String|简化模式下可用，返回的访问令牌。|
|tokenType|String|简化模式下可用，访问令牌类型。|
|expiresIn|Number|简化模式下可用，访问令牌过期时间，单位为秒，如果通过其他方式设置，则此处可能为空。|
|scope|String|简化模式下可用，实际权限范围，如果与申请一致，则此处可能为空。|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取帐号权限失败|

示例
```javascript
uni.account.authorize({
  type: "code",
  redirectUri: "http://www.example.com/",
  success: function(data) {
    console.log("handling success: " + data.code);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.account.getProfile(OBJECT)

获得用户基本信息。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|token|String|是|访问令牌|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|openid|String|用户的openid，可能为空|
|id|String|用户的user id，可能为空|
|unionid|String|用户在开放平台上的唯一标示符，本字段在满足一定条件下才会返回（需要在厂商的开放平台上额外申请）|
|nickname|String|用户的昵称，可能为空|
|avatar|Object|用户的头像图片地址，可能为空，按照分辨率组织，当只有一个分辨率时，可以使用default对应的图片地址|

unionid机制说明

如果开发者拥有多个移动应用，可通过unionid来区分用户的唯一性，因为只要是同一个开放平台帐号下的移动应用，用户的unionid是唯一的。换句话说，同一用户，对同一个开放平台下的不同应用，unionid是相同的。

示例
```javascript
uni.account.getProfile({
  token: "abcdefg",
  success: function(data) {
    console.log("handling success: " + data.nickname);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 支付

#### 接口声明

{"name": "service.pay"}

#### uni.pay.getProvider(OBJECT) 

获取服务提供商，如厂商的英文品牌名称，假如无此服务则返回空字符串。

参数

无。

返回值

字符串，服务提供商的代号。

示例
```javascript
uni.pay.getProvider();
```

#### uni.pay.pay(OBJECT)

使用支付完成付款。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|orderInfo|String|是|订单信息|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|result|String|支付结果|

fail 返回值

|参数名|类型|说明|
|:-|:-|:-|
|code|Number|返回状态码|
|message|String|消息内容|

fail异常码返回，不同的厂商提供的异常码会有差异，具体的异常码需要和厂商支付接口对接。

示例
```javascript
uni.pay.pay({
  orderInfo: "order1",
  success: function(data) {
    console.log("handling success: " + data.code);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 统计 

#### 接口声明

{"name": " service.stat"}

#### uni.stat.getProvider(OBJECT) 

获取服务提供商，如厂商的英文品牌名称，假如无此服务则返回空字符串。

参数

无。

返回值

字符串，服务提供商的代号。

示例
```javascript
uni.stat.getProvider();
```

#### uni.stat.recordCountEvent(OBJECT)

计数类型事件。通常⽤来描述⼀个事件累积发⽣的次数，适⽤的场景如按钮点击、界⾯进⼊、⽤⼾输⼊等。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|category|String|否|定义事件的类别.开发者可使⽤该参数对⾃定义打点做整理归类|
|key|String|是|定义事件的主键，作为该事件的唯⼀标识|
|map|Object|否|定义事件的属性和取值（Key-Value键值对）|

示例
```javascript
uni.stat.recordCountEvent({
  category: "Button_Click",
  key: "Button_OK_click"
});
```

#### uni.stat.recordCalculateEvent(OBJECT)

计算类型事件。通常⽤来描述⼀个带数值的事件的发⽣，适⽤的场景如⽤⼾消费事件，附带的数值是每次消费的⾦额；下载⽂件事件，附带的数值是每次下载消耗的时间等。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|category|String|否|定义事件的类别.开发者可使⽤该参数对⾃定义打点做整理归类|
|key|String|是|定义事件的主键，作为该事件的唯⼀标识|
|value|Number|是|定义事件的值。|
|map|Object|否|定义事件的属性和取值（Key-Value键值对）|

示例
```javascript
uni.stat.recordCalculateEvent({
  category: "user_pay",
  key: "buy_ebook",
  value: 20
});
```
