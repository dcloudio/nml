### 数据请求

#### 接口声明

{"name": "system.fetch"}

#### uni.fetch.fetch(OBJECT)

获取网络数据。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|url|String|是|资源url|
|data|String/Object|否|请求的参数，可以是字符串或者是 json 对象。|
|header|Object|否|请求的 header，会将其所有属性设置到请求的 header 部分。useragent 设置无效。示例：``{"Accept-Encoding": "gzip, deflate","Accept-Language": "zh-CN,en-US;q=0.8,en;q=0.6"}``|
|method|String|否|默认为 GET，可以是：OPTIONS，GET，HEAD，POST，PUT，DELETE，TRACE，CONNECT|
|success|Function|否|成功返回的回调函数|
|fail|Function|否|失败的回调函数，可能会因为权限失败|
|complete|Function|否|结束的回调函数（调用成功、失败都会执行）|

data 说明
- 如果是字符串，其值作为请求的 body，请求的 Content-Type 会被设置为 text/plain。
- 如果是 json 对象，会将其所有属性使用 urlencode 编码，组成一个字符串作为请求的 body，请求的 Content-Type 会被设置为 application/x-www-form-urlencoded。

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|code|Number|服务器状态 code|
|data|String|如果服务器返回的 header 中 type 是 text/* 或 application/json、application/javascript、application/xml，值是文本内容，否则是存储的临时文件的 uri。临时文件如果是图片或者视频内容，可以将图片设置到 image 或 video 控件上显示。|
|headers|Object|服务器 response 的所有 header|

示例
```javascript
uni.fetch.fetch({
  url: "http://www.example.com",
  success: function (data) {
    console.log("title: " + JSON.parse(data.data).title);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### 安全要求

对于HTTPS请求，对证书要求如下：

- HTTPS证书必须有效。证书必须被系统信任，不支持自签名证书，部署SSL证书的网站域名必须与证书颁发的域名一致，证书必须在有效期内。
- TLS必须支持1.1及以上版本。部分旧Android机型（4.4以下）还未支持 TLS1.1，请确保HTTPS服务器的TLS版本能够同时支持 TLS1.1 及以下版本，从安全性考虑，建议也支持TLS1.2，在配置加密套件时排除不安全算法：DES/3DES（除密钥K1≠K2≠K3外的场景）/SKIPJACK/RC2/MD2/MD4/SHA1。

部分CA可能不被操作系统信任，请开发者在选择证书时注意Android系统的相关通告。

### 上传下载 

#### 接口声明

{"name": "system.request"}

#### uni.request.upload(OBJECT)

上传文件。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|url|String|是|资源url|
|header|Object|否|请求的 header，会将其所有属性设置到请求的 header 部分。useragent 设置无效。|
|method|String|否|默认为 GET，可以是：POST，PUT|
|files|Array|是|需要上传的文件列表，使用 multipart/form-data 方式提交。files 参数是一个 file 对象的数组。|
|success|Function|否|成功返回的回调函数|
|fail|Function|否|失败的回调函数，可能会因为权限失败|
|complete|Function|否|结束的回调函数（调用成功、失败都会执行）|

files 参数说明

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|filename|String|否|multipart提交时，header中的文件名。|
|name|String|否|multipart提交时，表单的项目名，默认file。|
|uri|String|是|文件的本地地址。|
|type|String|否|文件的Content-Type格式，不填则会根据filename或uri的后缀从系统的映射表中查询对应的Content-Type，假如获取失败则报参数非法异常|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|code|Number|服务器状态code|
|data|String|如果服务器返回的header中type是text/*或application/json、application/javascript、application/xml，值是文本内容，否则是存储的临时文件的uri临时文件，如果是图片或者视频内容，可以将图片设置到image或video控件上显示|
|headers|Object|服务器response的所有header|

示例
```javascript
uni.request.upload({
  url: "http://www.example.com",
  files: [{
    uri: "internal://xxx/xxx/test",
    name: "file1",
    filename: "test.png"
  }],
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.request.download(OBJECT)

下载文件

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|url|String|是|资源url|
|header|Object|否|请求的header，会将其所有属性设置到请求的header部分。useragent设置无效。|
|success|Function|否|成功返回的回调函数|
|fail|Function|否|失败的回调函数|
|complete|Function|否|结束的回调函数（调用成功、失败都会执行）|

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|token|String|下载的token，根据此token获取下载状态|

示例

```javascript
uni.request.download({
  url: "http://www.example.com",
  success: function (data) {
    console.log("handling success" + data.token);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.request.onDownloadComplete(OBJECT)

监听下载任务。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|token|String|是|download接口返回的token|
|success|Function|否|成功返回的回调函数|
|fail|Function|否|失败的回调函数|
|complete|Function|否|结束的回调函数（调用成功、失败都会执行）|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|下载文件的uri，会存储在文件组织的mass分区下|

fail返回错误码

|错误码|说明|
|:-|:-|
|1000|下载失败|
|1001|下载任务不存在|

示例

```javascript
uni.request.onDownloadComplete({
  token: "123",
  success: function (data) {
    console.log("handling success" + data.uri);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```
