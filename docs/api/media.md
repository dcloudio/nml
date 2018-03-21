### 多媒体

#### 接口声明

{"name": "system.media"}

#### uni.media.takePhoto(OBJECT)

拍摄照片。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|cancel|Function|否|取消回调|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|选取的文件路径|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取相机权限失败。|

示例
```javascript
uni.media.takePhoto({
  success: function(data) {
    console.log("handling success: " + data.uri);
  }
});
```

#### uni.media.takeVideo(OBJECT)

拍摄视频。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|cancel|Function|否|取消回调|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|选取的文件路径|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取相机权限失败。|

示例
```javascript
uni.media.takeVideo({
  success: function(data) {
    console.log("handling success: " + data.uri);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.media.pickImage(OBJECT)

选择图片。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|cancel|Function|否|取消回调|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|选取的文件路径|

示例
```javascript
uni.media.pickImage({
  success: function(data) {
    console.log("handling success: " + data.uri);
  }
});
```

#### uni.media.pickVideo(OBJECT)

选择视频。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|cancel|Function|否|取消回调|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|选取的文件路径|

示例
```javascript
uni.media.pickVideo({
  success: function(data) {
    console.log("handling success: " + data.uri);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.media.compressImage(OBJECT) 

压缩指定路径的图片。

参数

|参数名|类型|默认值|必填|说明|
|:-|:-|:-|:-|:-|
|srcUri|String||是|源文件的uri|
|quality|Number||否|图片的压缩质量，0-100之间|
|radio|Number||否|尺寸压缩倍数，值越大，图片尺寸越小|
|format|String|JPEG|否|压缩格式，支持三种格式：JPEG，PNG，WEBP|
|success|Function||否|成功回调|
|fail|Function||否|失败回调|
|complete|Function||否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|dstUri|String|压缩成功后的文件路径uri|

fail 返回码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|
|301|文件路径不存在|

示例
```javascript
uni.media.compressImage({
  srcUri: "tmp://abc.jpg",
  quality: 80,
  radio: 2,
  format: "JPEG",
  success: function(data) {
    console.log(data.dstUri);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.media.getImageInfo(OBJECT) 

获取图片的信息。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|uri|String|是|源文件的uri|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|图片的文件路径uri|
|width|Number|图片的宽度，单位为px|
|height|Number|图片的高度，单位为px|
|size|Number|图片的大小，单位为Byte|

fail 返回码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|
|301|文件路径不存在|

示例
```javascript
uni.media.getImageInfo({
  uri: "tmp://abc.jpg",
  success: function(data) {
    console.log(data.uri + data.width + data.height + data.size);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.media.editImage(OBJECT) 

编辑图片，系统提供基础基于矩形框的裁剪图片UI组件，返回裁剪后保存的图片路径。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|srcUri|String|是|源文件的uri，仅支持本地图片路径|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|dstUri|String|裁剪后图片的文件路径uri|

fail 返回码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|
|301|文件路径不存在|

示例
```javascript
uni.media.cropImage({
  srcUri: "tmp://abc.jpg",
  success: function(data) {
    console.log(data.dstUri);
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.media.cropImage (OBJECT) 

根据输入的参数，在本地裁切图片。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|uri|String|是|图片本地地址。|
|cropData|Object|是|裁切参数：{ x: 0,  y: 0,  width: 240,  height: 180,  rotate: 0,  scale: { x: 1, y: 1 }}|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|裁切后的图片地址。|

fail 返回码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|
|301|文件路径不存在|

示例
```javascript
uni.media.cropImage({
  success: function(ret) {
    console.log(ret);
  },
  uri: "[image location]",
  cropData: {
    x: 0,
    y: 0,
    width: 240,
    height: 180,
    rotate: 0,
    scale: {
      x: 1,
      y: 1
    }
  }
});
```
