### 数据存储

#### 接口声明

{"name": "system.storage"}

#### uni.storage.get(OBJECT)

读取存储内容。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|key|String|是|索引|
|default|String|否|如果key不存在，返回default。如果default未指定，返回长度为0的空字符。|
|success|Function|否|成功回调，其返回值为key对应的存储内容。|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.storage.get({
  key: 'A1',
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.storage.set(OBJECT)

修改存储内容。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|key|String|是|索引|
|value|String|否|新值。如果新值是长度为0的空字符串，会删除以key为索引的数据项。|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.storage.set({
  key: 'A1',
  value: 'V1',
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.storage.clear(OBJECT)

清空存储内容。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.storage.clear({
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.storage.delete(OBJECT) 

删除存储内容。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|key|String|是|索引|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
Uni.storage.delete({
  key: 'A1',
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 文件存储 

#### 接口声明

{"name": "system.file"}

#### uni.file.move(OBJECT)

将源文件移动到指定位置，接口中使用的URI描述请参考"文件组织"。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|srcUri|String|是|源文件的uri，不能是应用资源路径和tmp类型的uri|
|dstUri|String|是|目标文件的uri，不能是应用资源路径和tmp类型的uri|
|success|Function|否|成功回调，返回目标文件的uri|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

fail 返回错误码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|

示例
```javascript
uni.file.move({
  srcUri: "internal://cache/path/to/file",
  dstUri: "internal://files/path/to/file",
  success: function (uri) {
    console.log("move success: " + uri);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.file.copy(OBJECT)

将源文件复制一份并存储到指定位置，接口中使用的URI描述请参考"文件组织"。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|srcUri|String|是|源文件的uri|
|dstUri|String|是|目标文件的uri，不能是应用资源路径和tmp类型的uri|
|success|Function|否|成功回调，返回目标文件的uri|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

fail 返回错误码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|

示例
```javascript
uni.file.copy({
  srcUri: "internal://cache/path/to/file",
  dstUri: "internal://files/path/to/file",
  success: function (uri) {
    console.log("copy success: " + uri);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.file.list(OBJECT)

获取指定目录下的文件列表，接口中使用的URI描述请参考"文件组织"。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|uri|String|是|目录uri，不能是应用资源路径和tmp类型的uri|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|fileList|Array|文件列表，每个文件的格式为``{uri:'file1',lastModifiedTime:1234456, length:123456}``|

每个文件的元信息

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|文件的uri，该uri可以被其他组件或Feature访问。|
|length|Number|文件大小，单位B。|
|lastModifiedTime|Number|文件的保存是的时间戳，从 1970/01/01 00:00:00 GMT 到当前时间的毫秒数。|

fail 返回错误码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|

示例
```javascript
uni.file.list({
  uri: "internal://files/movies/",
  success: function (data) {
    console.log(data.fileList)
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.file.get(OBJECT)

获取本地文件的文件信息，接口中使用的URI描述请参考"文件组织"。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|uri|String|是|目标文件的uri，不能是应用资源路径和tmp类型的uri|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|uri|String|文件的uri，该uri可以被其他组件或Feature访问。|
|length|Number|文件大小，单位B。|
|lastModifiedTime|Number|文件的保存是的时间戳，从 1970/01/01 00:00:00 GMT 到当前时间的毫秒数。|

fail 返回错误码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|

示例
```javascript
uni.file.get({
  uri: 'internal://files/path/to/file',
  success: function (data) {
    console.log(data.uri);
    console.log(data.length);
    console.log(data.lastModifiedTime);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.file.delete(OBJECT)

删除本地存储的文件，接口中使用的URI描述请参考"文件组织"。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|uri|String|是|需要删除的文件uri，不能是应用资源路径和tmp类型的uri|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

fail 返回错误码

|错误码|说明|
|:-|:-|
|202|参数错误|
|300|I/O错误|

示例
```javascript
uni.file.delete({
  uri: 'internal://files/path/to/file',
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```
