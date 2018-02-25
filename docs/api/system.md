### 二维码

#### 接口声明

{"name": "system.barcode"}

#### uni.barcode.scan(OBJECT)

扫描二维码。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|cancel|Function|否|取消回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|result|String|解析后的内容|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取相机权限失败|

示例
```javascript
uni.barcode.scan({
  success: function (data) {
    console.log("handling success: " + data.result);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 传感器

#### 接口声明

{"name": "system.sensor"}

#### uni.sensor.subscribeAccelerometer(OBJECT)

监听重力感应数据。如果多次调用，仅最后一次调用生效。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|callback|Function|是|重力感应数据变化后会回调此函数，5次每秒。|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|x|Number|x轴坐标|
|y|Number|y轴坐标|
|z|Number|z轴坐标|

示例
```javascript
uni.sensor.subscribeAccelerometer({
  callback: function (ret) {
    console.log("handling callback");
  }
});
```

#### uni.sensor.unsubscribeAccelerometer()

取消监听重力感应数据。

参数

无。

示例
```javascript
uni.sensor.unsubscribeAccelerometer();
```

#### uni.sensor.subscribeCompass(OBJECT)

监听罗盘数据。如果多次调用，仅最后一次调用生效。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|callback|Function|是|重力感应数据变化后会回调此函数，5次每秒。|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|direction|Float|面对的方向度数|

示例
```javascript
uni.sensor.subscribeCompass({
  callback: function (ret) {
    console.log("handling callback");
  }
});
```

#### uni.sensor.unsubscribeCompass()

取消监听罗盘数据。

参数

无。

示例
```javascript
uni.sensor.unsubscribeCompass();
```

#### uni.sensor.subscribeProximity(OBJECT) 

监听距离感应数据。如果多次调用，仅最后一次调用生效。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|callback|Function|是|距离感应数据变化后会回调此函数。|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|distance|Number|手机距离，单位为cm。|

示例
```javascript
uni.sensor.subscribeProximity({
  callback: function (ret) {
    console.log("handling callback");
  }
});
```

#### uni.sensor.unsubscribeProximity()

取消监听距离感应数据。

参数

无。

示例
```javascript
uni.sensor.unsubscribeProximity();
```

#### uni.sensor.subscribeLight(OBJECT) 

监听光线感应数据。如果多次调用，仅最后一次调用生效。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|callback|Function|是|光线感应数据变化后会回调此函数。|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|intensity|Number|光线强度，单位为lux。|

示例
```javascript
uni.sensor.subscribeLight({
  callback: function (ret) {
    console.log("handling callback");
  }
});
```

#### uni.sensor.unsubscribeLight()

取消监听光线感应数据。

参数

无。

示例
```javascript
uni.sensor.unsubscribeLight();
```

### 剪贴板

#### 接口声明

{"name": "system.clipboard"}

#### uni.clipboard.set(OBJECT)

修改剪贴板内容。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|text|String|是|需要放到剪切板的内容|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.clipboard.set({
  text: 'text'
});
```

#### uni.clipboard.get(OBJECT)

读取剪贴板内容。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|text|String|剪切板内容|

示例
```javascript
uni.clipboard.get({
  success: function (data) {
    console.log("handling success: " + data.text);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 地理位置

#### 接口声明

{"name": "system.geolocation"}

#### uni.geolocation.getLocation(OBJECT)

获取地理位置。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|timeout|Long|否|设置超时时间，单位是ms，默认值为1000。在权限被系统拒绝或者定位设置不当的情况下，有可能永远不能返回结果，因而需要设置超时。超时后会使用fail回调|
|success|Function|是|成功回调|
|fail|Function|否|失败回调，原因可能是用户拒绝。|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|longitude|Float|经度|
|latitude|Float|经度|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取定位权限失败|
|1000|系统位置开关关闭|

示例
```javascript
uni.geolocation.getLocation({
  success: function (data) {
    console.log("handling success: longitude=" + data.longitude + ",    latitude = " + data.latitude);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.geolocation.subscribe(OBJECT)

监听地理位置。如果多次调用，仅最后一次调用生效。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|callback|Function|是|每次位置信息发生变化，都会被回调。|
|fail|Function|否|失败回调，原因可能是用户拒绝。|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|longitude|Float|经度|
|latitude|Float|经度|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取定位权限失败|
|204|超时返回|
|1000|系统位置开关关闭|

示例
```javascript
uni.geolocation.subscribe({
  callback: function (data) {
    console.log("handling success: longitude=" + data.longitude + ", latitude = " + data.latitude);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.geolocation.unsubscribe()

取消监听地理位置。

参数

无。

示例
```javascript
uni.geolocation.unsubscribe();
```

### 日历事件

#### 接口声明

{"name": "system.calendar"}

#### uni.calendar.insert(OBJECT)

插入日历事件。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|title|String|是|事件的标题。|
|description|String|否|事件的描述。|
|startDate|Long|是|事件开始时间，以从公元纪年开始计算的协调世界时毫秒数表示。|
|endDate|Long|是|事件结束时间，以从公元纪年开始计算的协调世界时毫秒数表示。|
|timezone|String|否|事件的时区|
|allDay|Boolean|否|true 表示此事件占用一整天（按照本地时区的定义）。false 表示它是常规事件，可在一天内的任何时间开始和结束。|
|rrule|String|重复事件必须|事件的重复发生规则格式。例如，"FREQ=WEEKLY;COUNT=10;WKST=SU"。|
|remindMinutes|Array|否|在事件开始前几分钟进行提醒。例如：[5,15,30]。|
|organizer|String|否|事件组织者（所有者）的电子邮件。|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取写日历权限失败。|
|202|参数非法，如输入时间格式不对、参数不符合标准。|

示例
```javascript
uni.calendar.insert({
  title: "事件Ａ",
  startDate: "1490770543000",
  endDate: "1490880543000",
  remindMinutes: [5, 15, 30],
  duration: "PT1H",
  rrule: "FREQ=WEEKLY;COUNT=２",
  success: function (data) {
    console.log("handling success");
  }
});
```

### 桌面图标 

#### 接口声明

{"name": "system.shortcut"}

#### uni.shortcut.hasInstalled(OBJECT)

获取桌面图标是否创建。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调。true：已创建。false：未创建。|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.shortcut.hasInstalled({
  success: function () {
    console.log("handling success");
  }
});
```

#### uni.shortcut.install(OBJECT)

创建桌面图标。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取创建桌面图标权限失败。|

示例
```javascript
uni.shortcut.install({
  success: function () {
    console.log("handling success");
  }
});
```

### 网络状态

#### 接口声明

{"name": "system.network"}

#### uni.network.getType(OBJECT)

获取网络类型。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|metered|Boolean|是否按照流量计费。|
|type|String|网络类型，可能的值为2g，3g，4g，wifi，none。|

示例
```javascript
uni.network.getType({
  success: function (data) {
    console.log("handling success: " + data.type);
  }
});
```

#### uni.network.subscribe(OBJECT)

监听网络连接状态。如果多次调用，仅最后一次调用生效。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|callback|Function|否|每次网络发生变化，都会被回调。|
|fail|Function|否|失败回调，可能是因为缺乏权限。|

callback 返回值

|参数名|类型|说明|
|:-|:-|:-|
|metered|Boolean|是否按照流量计费。|
|type|String|网络类型，可能的值为2g，3g，4g，wifi，none。|

示例
```javascript
uni.network.subscribe({
  callback: function (data) {
    console.log("handling callback");
  }
});
```

#### uni.network.unsubscribe()

取消监听网络连接状态。

参数

无。

示例
```javascript
uni.network.unsubscribe();
```

### 设备信息

#### 接口声明

{"name": "system.device"}

#### uni.device.getInfo(OBJECT)

获取设备信息。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|brand|String|设备品牌|
|manufacturer|String|设备生产商|
|model|String|设备型号|
|product|String|设备代号|
|osType|String|操作系统名称|
|osVersionName|String|操作系统版本名称|
|osVersionCode|Number|操作系统版本号|
|platformVersionName|String|运行平台版本名称|
|platformVersionCode|Number|运行平台版本号|
|language|String|系统语言|
|region|String|系统地区|
|screenWidth|Number|屏幕宽|
|screenHeight|Number|屏幕高|

示例
```javascript
uni.device.getInfo({
  success: function (ret) {
    console.log("handling success");
  }
});
```

#### uni.device.getId(OBJECT)

获取设备Id。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|type|Array|是|支持device、mac、user三种类型，可以同时提供多个，至少提供一个。|
|success|Function|否|成功回调。每项结果仅在type中有对应类型的时候才会返回mac，在Android M及以上返回固定值：02:00:00:00:00:00。|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|device|String|设备唯一标识。|
|mac|String|设备的mac地址。|
|user|String|用户唯一标识。|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取电话状态权限失败|

示例
```javascript
uni.device.getId({
  type: ["device", "mac"],
  success: function (data) {
    console.log("handling success: " + data.device);
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 屏幕亮度 

#### 接口声明

{"name": "system.brightness"}

#### uni.brightness.getValue(OBJECT)

获得当前屏幕亮度值。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|value|Number|屏幕亮度，取值范围0-255|

示例
```javascript
uni.brightness.getValue({
  success: function (ret) {
    console.log("handling success");
  }
});
```

#### uni.brightness.setValue(OBJECT)

设置当前屏幕亮度值。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|value|Number|是|屏幕亮度，取值范围0-255|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.brightness.setValue({
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

#### uni.brightness.getMode(OBJECT)

获得当前屏幕亮度模式。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|mode|Number|0为手动调节屏幕亮度，1为自动调节屏幕亮度。|

示例
```javascript
uni.brightness.getMode({
  success: function (ret) {
    console.log("handling success");
  }
});
```

#### uni.brightness.setMode(OBJECT)

设置当前屏幕亮度模式。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|mode|Number|是|0为手动调节屏幕亮度，1为自动调节屏幕亮度。|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

示例
```javascript
uni.brightness.setMode({
  success: function (data) {
    console.log("handling success");
  },
  fail: function (data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 电量信息 

#### 接口声明

{"name": "system.battery"}

#### uni.battery.getInfo(OBJECT)

获取当前设备的电量信息。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|charging|Boolean|是否正在充电。|
|level|Float|当前电量，0.0 - 1.0 之间。|

示例
```javascript
uni.battery.getInfo({
  success: function (ret) {
    console.log("handling success");
  }
});
```

### 录音 

#### 接口声明

{"name": "system.record"}

#### uni.record.startRecord(OBJECT)

开始录音。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

success 返回值

|参数名|类型|说明|
|:-|:-|:-|
|path|String|录音文件的存储路径，会记录在应用的缓存目录中。|

fail 返回错误码

|错误码|说明|
|:-|:-|
|201|用户拒绝，获取录音权限失败|

示例
```javascript
uni.record.startRecord({
  success: function (data) {
    console.log("handling success: " + data.uri);
  }
});
```

#### uni.record.stopRecord(OBJECT)

停止录音。

参数

无。

示例
```javascript
uni.record.stopRecord();
```

### 系统音量

#### 接口声明

{"name": "system.volume"}

#### uni.volume.getMediaMaxVolume()

获取当前多媒体最大音量。

参数

无。

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|maxVolume|Number|系统媒体最大音量。|

#### uni.volume.getMediaMinVolume()

获取当前多媒体最小音量。

参数

无。

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|minVolume|Number|系统媒体最小音量。|

#### uni.volume.getMediaVolume(OBJECT)

获取当前多媒体音量。

参数

无。

success返回值

|参数名|类型|说明|
|:-|:-|:-|
|currentVolume|Number|系统媒体当前音量。|

#### uni.volume.setMediaVolume(OBJECT)

设置当前多媒体音量。

参数

|参数名|类型|必填|说明|
|:-|:-|:-|:-|
|volume|Number|是|设置的音量|
|success|Function|否|成功回调|
|fail|Function|否|失败回调|
|complete|Function|否|执行结束后的回调|

fail 返回错误码

|错误码|说明|
|:-|:-|
|200|一般性错误|
|202|参数非法|
