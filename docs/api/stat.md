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