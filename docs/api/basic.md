## 基本功能
### 应用上下文
#### 接口声明

无需声明。

#### uni.app.getInfo()

获取当前应用信息。

参数

无。

返回值

|参数名|类型|说明|
|:-|:-|:-|
|name|String|应用名称|
|versionName|String|应用版本名称|
|versionCode|Number|应用版本号|
|logLevel|String|log级别|
|source|Object|应用来源|

source 说明

|参数名|类型|说明|
|:-|:-|:-|
|packageName|String|来源 app 的包名，一级来源|
|type|String|来源类型，二级来源，值为 shortcut、push、url、barcode、nfc、bluetooth、other。|
|extra|Object|来源其他信息，与 type 相关，不同的 type，extra 中的字段会不同。|

extra的相关说明如下：

- type=shortcut
  - scene：三级来源，表示快捷方式创建的场景，值为dialog（平台内部策略Dialog弹窗创建）、api（API接口调用创建）、web（H5站接入流量切换，浏览时创建）、other
  - original：原始来源 source，表示快捷方式创建时的来源

示例
```javascript
console.log(JSON.stringify(uni.app.getInfo()));
```

### 日志打印

#### 接口声明

无需声明。

#### console.debug/log/info/warn/error(message)

打印一段文本。

参数

|参数|类型|必填|说明|
|:-|:-|:-|:-|
|message|String|是|要打印的文本，也可以是格式化文本，规则与浏览器的console相同|

示例
```javascript
for (var i = 0; i < 5; i++) {
  console.log("Hello, %s. You've called me %d times.", "Bob ", i + 1);
}
console.log("I want to print a number: %d", 123456);
```

### 页面路由

#### 接口声明

无需声明。

#### uni.router.push(OBJECT)

跳转到应用内的某个页面。

参数

|参数|类型|必填|说明|
|:-|:-|:-|:-|
|uri|String|是|详细参考下文|
|params|Object|否|跳转时需要传递的数据，参数可以在页面中通过this.param1的方式使用，param1为json中的参数名，param1对应的值会统一转换为String类型。|

要跳转到的uri，可以是下面的格式：
1. 包含schema的完整uri；目前支持的schema有tel，sms和mailto，例如tel:10086，mailto:abc@gmail.com
2. 以"/"开头的应用内页面的路径；例：/about
3. 以非"/"开头的应用内页面的名称；例：About
4. 特殊的，如果uri的值是"/",则跳转到path为"/"的页，没有则跳转到首页。

支持包含schema的完整uri。对于带有schema的uri，处理流程如下：
1. 查找app下所有page的filter设置来选择合适的page处理请求（参见manifest文件）
2. 如果没有合适的page能够处理请求，会使用默认策略来处理请求。目前默认策略支持对http、https、internal这几种schema的处理
3. 如果默认策略也不能处理请求，会尝试使用系统中的应用来处理请求
4. 如果没有系统应用可以处理请求，会抛弃请求

默认策略的处理逻辑：
1. 如果schema是http/https，会用内置的web页面打开网页
2. 如果schema是internal（参见“文件组织”），会根据uri的文件扩展名来确定文件类型，再调用系统中的应用打开文件

示例
```javascript
// launch phone app  
uni.router.push({
  uri: 'tel:10086'
});
// open page by path  
uni.router.push({
  uri: '/about',
  params: {
    testId: '1'
  }
});
// open page by name  
uni.router.push({
  uri: 'About',
  params: {
    testId: '1'
  }
});

```

新增下面的用法：
```javascript
// open web page  
uni.router.push({
  uri: 'http://www.example.com '
});
// install apk  
uni.router.push({
  uri: 'internal://cache/example.apk '
});

```

#### uni.router.replace(OBJECT)

跳转到应用内的某个页面，当前页面无法返回。

参数

|参数|类型|必填|说明|
|:-|:-|:-|:-|
|uri|String|是|详细参考下文|
|params|Object|否|跳转时需要传递的数据，参数可以在页面中通过this.param1的方式使用，param1为json中的参数名，param1对应的值会统一转换为String类型。|

要跳转到的uri，可以是下面的格式：
1. 以"/"开头的应用内页面的路径；例：/about
2. 以非"/"开头的应用内页面的名称；例：About
3. 特殊的，如果uri的值是"/"，则跳转到path为"/"的页，没有则跳转到首页。

示例
```javascript
uni.router.replace({
  uri: '/test',
  params: {
    testId: '1'
  }
})

```

#### uni.router.back()

返回上一页面。

参数

无。

示例
```javascript
// A页面  
uni.router.push({
  uri: 'B'
})

// B页面  
uni.router.push({
  uri: 'C'
})

// C页面通过back，将返回B页面  
uni.router.back();
// B页面通过back，将返回A页面  
uni.router.back();
```

#### uni.router.clear()

清空所有历史页面记录，仅保留当前页面，即保留栈顶页面。

参数

无。

示例
```
uni.router.clear();
```