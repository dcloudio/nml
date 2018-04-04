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