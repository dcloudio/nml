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