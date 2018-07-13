### 微信支付 

#### 接口声明

{"name": "service.wxpay"}

#### uni.wxpay.getType()

获取当前可用的微信支付调用方式。

参数

无。

返回值

|返回值|备注|
|:-|:-|
|none|微信未安装|
|APP|微信app调用方式，服务端向微信支付下单时，trade_type需要是APP，参考"微信app支付"。|
|MWEB|微信网页调用方式，服务端向微信支付下单时，trade_type需要是MWEB，参考"微信网页支付"。|

#### uni.wxpay.pay(OBJECT)

发起微信支付。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|prepayid|String|是|微信支付服务器生成的预支付订单id，参考"微信app支付"和"微信网页支付"。|
|extra|Object|是|当前支付方式下，需要填入的额外订单信息。|
|success|Function|否|成功后的回调函数。App方式下，回调发生在用户支付完成之后。网页方式下，回调发生在订单提交给微信app之后。|
|fail|Function|否|失败回调|
|cancel|Function|否|取消回调|

extra参数app方式

|字段名|必选|说明|
|:-|:-|:-|
|app_id|是|微信支付订单中的app_id。|
|partner_id|是|微信支付订单中的partner_id。|
|package_value|是|微信支付订单中的package_value。|
|nonce_str|是|微信支付订单中的nonce_str。|
|time_stamp|是|微信支付订单中的time_stamp。|
|order_sign|是|微信支付订单中的order_sign。|

extra参数网页方式

|字段名|必选|说明|
|:-|:-|:-|
|mweb_url|否|在微信的支付服务器下单以后，微信返回的MWEB_URL，在CP用于微信支付的h5页面中，直接将mweb_url取出后跳转过去即可，但这个做法并不是强制的，您也可以通过其他自定义键值向您自己的服务器换取MWEB_URL。|
|custome_key|否|其他的自定义键值，cp可以根据需要增加其他自己认为需要的键名和键值。|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|prepayid|String|只在App支付方式下会出现，微信支付订单的prepayId。|
|final_url|String|只在网页方式下会出现，拼接参数之后，最终用于打开网页的url。|

fail 返回错误码

|错误码|说明|
|:-|:-|
|900|在manifest.json中配置的应用签名有误，无法解析|
|901|在manifest.json中配置的应用包名有误|
|1000|微信未安装|
|1001|用于微信网页支付的url配置找不到|
|2001|订单已经提交给微信，但是微信返回错误, 可能的原因：签名错误、未注册APPID、项目设置APPID不正确、注册的APPID与设置的不匹配、其他异常等。|

示例
```javascript
useWxpay: function() {
  var payType = uni.wxpay.getType();
  if(payType == 'APP') {
    uni.wxpay.pay({
      //微信 app支付的prepayId 
      prepayid: 'your order prepayid,eg: wx20170101abcdef1234567890',
      extra: {
        app_id: 'your app_id',
        partner_id: 'your partner_id',
        package_value: 'your package_value',
        nonce_str: 'your nonce_str',
        time_stamp: 'your time_stamp',
        order_sign: 'your order sign'
      },
      fail: function(data, code) {
        console.log("WX PAY failed, code=" + code);
      },
      cancel: function(data) {
        console.log("WX PAY cancelled by user");
      },
      success: function(data) {
        console.log("WX PAY success");
      }
    });
  } else if(payType == 'MWEB') {
    uni.wxpay.pay({
      //微信网页支付的prepayId 
      prepayid: 'your order prepayid,eg: wx20170101abcdef1234567890',
      extra: {
        //传递给支付页面的自定义参数, 根据需要进行设置,会被urlEncode之后拼接在配置的url尾部 
        mweb_url: 'https://wx.tenpay.com/cgi-bin/mmpayweb-bin',
        customeKey1: 'customeValue1',
        customeKey2: 'customeValue2'
      },
      fail: function(data, code) {
        console.log("WX H5 PAY handling fail, code=" + code);
      },
      cancel: function(data) {
        console.log("WX H5 PAY handling cancel");
      },
      success: function(data) {
        //H5方式下，支付成功的回调仅仅只是指将订单递交给微信，并不意味着支付已经成功完成 
        console.log("WX H5 PAY handling success");
      }
    });
  } else {
    console.log("wx pay not avaliable.");
  }
}
```

#### 接入说明

方式选择

app支付只在部分手机系统上被支持，而网页支付在所有手机系统上均能被支持，但是网页支付产生支付回调的时机发生在订单提交给微信之后，并非和app支付那样发生在支付产生结果的时候。推荐cp同时支持两种接入支付方式。

app支付接入

1. 参考"微信app支付"，完成微信支付服务端的接入。
2. 在微信支付后台添加一个新的app，该app需要有一个新的包名，不要和自己android app应用的包名冲突，不需要对外发布，但是需要提交一个apk以注册新的包名对应的app签名。
3. 将新注册的app包名和app签名（签名的值可以在安装新注册的apk后使用这个工具获取）的值，分别填在manifest.json微信支付接口声明的package和sign两个字段中，如接口声明中所示。

网页支付接入

1. 参考"微信网页支付"，完成微信支付服务端的接入。
2. 在服务端提供一个发起支付的H5页面，该页面读取传入的mweb_url，然后自动跳转到mweb_url。将步骤2中支付H5页面对应的url填在manifest.json微信支付接口声明的url字段中，如接口声明中所示。

### 支付宝支付

#### 接口声明
{"name": "service.alipay"}

#### uni.alipay.pay(OBJECT)

使用支付宝支完成支付。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|orderInfo|String|是|服务端生成的订单信息，参考支付宝的"请求参数说明文档"。|
|callback|Function|否|支付结果回调，格式参考支付宝的"通知参数说明文档"。|

示例
```javascript
uni.alipay.pay({
  orderInfo: "order1",
  callback: function(ret) {
    console.log("handling callback");
  }
});
```

### 第三方分享 

#### 接口声明 {#接口声明-29}

{"name": "service.share"}

#### uni.share.getProvider(OBJECT) 

获取服务提供商，如厂商的英文品牌名称，假如无此服务则返回空字符串。

参数

无。

返回值

字符串，服务提供商的代号。

示例
```javascript
uni.share.getProvider();
```

#### 

#### uni.share.share(OBJECT)

分享内容。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|shareType|Int|是|分享类型。默认图文0，纯文字1，纯图片2，英文3，视频4。|
|title|String|分享类型0,1,3,4必须|分享的标题。|
|summary|String|否|分享的摘要。|
|targetUrl|String|分享类型0,3,4必须|点击后的跳转URL。|
|imagePath|String|分享类型2,3,4必须|分享图片/缩略图的本地地址。|
|mediaUrl|String|分享类型3,4必须|分享的音乐/视频数据URL。|
|success|Function|否|成功回调（暂不支持）。|
|fail|Function|否|失败回调|
|cancel|Function|否|取消回调|

示例
```javascript
uni.share.share({
  shareType: 0,
  title: "我是标题",
  summary: "我是摘要",
  imagePath: "xxx/xxx/xxx/share.jpg",
  targetUrl: "http://www.example.com",
  success: function(data) {
    console.log("handling success");
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### 接入说明

1. 参考第三方平台文档进行接入（微信开放平台/QQ开放平台/微博开放平台）。
2. 在第三方平台后台添加一个新的app，该app包名需要与manifest.json中的package一致。
3. 将app签名（签名的值获取：生成一个apk使用第三方后台注册的签名签名apk，在安装apk后使用这个工具获取）的值和各个平台获取的应用ID，填在manifest.json分享接口声明的appSign和对应的xxKey字段中，如上文
4. 支持手Q、微信、微博分享，暂时无法获取分享结果。
5. 推荐使用图文分享类型，其他类型个别平台无法支持,将使用图文类型分享。
6. 分享图片仅支持本地图片。