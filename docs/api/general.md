## 通用规则
接口方法有同步、异步、回调、异步回调四种类型，不同类型的接口方法，提供的能力也不相同。
### 同步

同步接口会直接返回调用结果，结果可以是任意类型，参见相关的接口文档。

使用示例
```
console.log(JSON.stringify(uni.app.getInfo()));
```

### 异步

异步接口不会立即返回调用结果，开发者需要提供合适的回调函数，异步接口会在任务结束时调用相应的回调函数。

异步接口支持的回调函数

|回调函数|参数名|类型|必填|说明|
|:-|:-|:-|:-|:-|
|success|data|Any|调用结果，可以是任意类型，参见接口使用文档。|在调用成功时回调|
|fail|data|Any|错误信息，一般是描述错误信息的字符串，也可以是其他类型，参见接口使用文档。|在调用失败时回调|
||code|Number|错误代码，如果文档未特别说明，会返回200。如果返回其他错误代码，需要在文档中列举说明。|在调用失败时回调|
|cancel|data|Any|调用结果，一般无内容，参见接口使用文档。|在用户取消时回调。部分需要用户交互的接口调用中可能提供此回调支持|
|complete|无|无|无|在调用完成时回调|

success、fail和cancel三个回调函数是互斥的，每次接口调用都会且仅会调用这三个回调函数中的一个，之后会再调用一次complete回调。

使用示例
```javascript
uni.prompt.showContextMenu({
  itemList: ['item1', 'item2'],
  itemColor: '#ff33ff',
  success:  function  (data) {
    console.log("handling callback");
  },
  fail:  function  (data, code) {
    console.log("handling fail, code=" + code);
  },
  cancel:  function  (data) {
    console.log("handling cancel");
  },
  complete:  function  () {
    console.log("handling complete");
  },
});

```

### 回调

同异步接口一样，回调接口也不会立即返回调用结果，开发者需要提供合适的回调函数，回调接口会调用相应的回调函数。不同的是，回调接口可能会多次调用回调函数返回结果。

回调接口支持的回调函数

|回调函数|参数名|类型|必填|说明|
|:-|:-|:-|:-|:-|
|callback|data|Any|调用结果，可以是任意类型，参见接口使用文档。|在每次获得结果时回调，可能会回调多次。|
|fail|data|Any|错误信息，一般是描述错误信息的字符串，也可以是其他类型，参见接口使用文档。|调用失败时回调。一旦调用此回调，callback调用不会再被调用，接口调用结束。|
||code|Number|错误代码，如果文档未特别说明，会返回200。如果返回其他错误代码，需要在文档中列举说明。|调用失败时回调。一旦调用此回调，callback调用不会再被调用，接口调用结束|

以地理位置接口（uni.system.geolocation）为例：

- 如果调用 ``uni.geolocation.subscribe`` 监听地理位置的改变，则每次地理位置发生改变，都会调用传入的callback回调，返回新的位置信息；
- 如果因为用户拒绝授予权限，会调用fail回调函数并结束接口调用，callback回调函数永远不会被调用。

使用示例

```javascript
uni.geolocation.subscribe({
  callback: function(ret) {
    console.log("handling callback");
  },
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});
```

### 异步回调

同异步接口一样，异步回调接口不会立即返回调用结果，开发者需要提供合适的回调函数，接口在执行完成时会调用回调函数返回结果。

同异步接口不同的是，这类接口仅接受一个callback回调，调用成功与否需要在callback回调中判断返回的结果来确定。callback仅会被回调一次。

异步回调接口支持的回调函数

|回调函数|参数名|类型|必填|说明|
|:-|:-|:-|:-|:-|
|callback|data|Any|调用结果，可以是任意类型，参见接口使用文档。|在获得结果时回调，会且仅会回调一次。|

使用示例
```javascript
uni.alipay.pay({
  orderInfo: "order1",
  callback: function(ret) {
    console.log("handling callback");
  }
});
```

### 正确实现回调函数

除同步接口外，异步接口、回调接口、异步回调接口都不会在调用后立即返回结果，而是一段时间后执行回调函数并携带返回结果作为参数。同时，执行接口调用所处的页面与最终执行回调函数的页面可能是两个不同的页面。

假设执行接口调用的页面为A，那么最后执行回调函数时，有可能处于以下三种状态中的一种：
- A页面仍然在显示中
- A页面被切换到后台，当前显示的是页面B
- A页面已经被销毁。

回调函数需要判断页面的状态，并根据状态做出正确的处理。页面拥有$valid和$visible属性，为布尔值，以方便判断页面状态：
```javascript
// 页面是否有效（没有被销毁）
this.$valid
// 页面是否处于后台
this.$visible
```

举个例子：有个页面需要展示用户当前地理位置的商户列表，需要先调用 ``uni.geolocation.getLocation()`` 接口获取当前的位置信息，然后调用 ``uni.fetch.fetch()`` 接口获取服务列表。在实现 ``uni.geolocation.getLocation()`` 的回调函数时，就需要判断当前页面的状态进行不同的操作：

- 如果页面仍然在显示中，继续调用fetch接口获取服务列表
- 如果页面已被切换到后台，则缓存当前的地理位置
- 如果页面已被销毁，则不做任何处理

代码示例
```javascript
uni.geolocation.getLocation({
  success: function(data) {
    if (this.$valid && this.$visible) {
      // 页面处于当前显示中
    } else if (this.$valid && !this.$visible) {
      // 页面处于后台
    } else {
      // 页面处于被销毁
    }
  }.bind(this),
  fail: function(data, code) {
    console.log("handling fail, code=" + code);
  }
});

```

需要特别注意的是，回调接口的回调函数会被多次调用。如果在页面切换到后台或者被销毁后，回调函数仍然被多次回调的话，既会影响应用的响应速度，也会造成内存泄露。因此，强烈建议使用下面任一种方法取消回调接口调用：

1. 在页面的onHide函数或onDestroy函数中取消回调接口调用
2. 在回调函数中判断当前页面的状态，发现页面被切换到后台或销毁后取消回调接口调用

推荐使用第1种方法，因为它能更及时的取消回调，最好的避免额外消耗。

举个例子，有个页面调用 ``uni.geolocation.subscribe`` 记录用户的运动轨迹。

使用第1种方法取消回调接口调用的逻辑如下：

- onDestroy函数中调用geolocation.unsubscribe取消订阅
- 在回调函数中判断当前页面的状态并正确处理

代码示例：
```javascript
export default {
  onDestroy() {
    uni.geolocation.unsubscribe()
  },
  record() {
    uni.geolocation.getLocation({
      success: function(data) {
        if (this.$valid && this.$visible) {
          // 页面处于当前显示中，记录轨迹  
        } else if (this.$valid && !this.$visible) {
          // 页面处于后台，记录轨迹  
        } else {
          // 页面处于被销毁，不做处理  
        }
      }.bind(this),
      fail: function(data, code) {
        console.log("handling fail, code= " + code);
      }
    })
  }
};

```

使用第2种方法取消回调接口调用的逻辑如下：

- 在uni.geolocation.subscribe调用传入的success回调函数中，首先判断页面状态
- 如果页面已经被销毁，调用uni.geolocation.unsubscribe取消订阅

代码示例：
```javascript
uni.geolocation.getLocation({
  success: function(data) {
    if (this.$valid && this.$visible) {
      // 页面处于当前显示中，记录轨迹  
    } else if (this.$valid && !this.$visible) {
      // 页面处于后台，记录轨迹  
    } else {
      // 页面处于被销毁  
      uni.geolocation.unsubscribe()
    }
  }.bind(this),
  fail: function(data, code) {
    console.log("handling fail, code= " + code);
  }
});

```

在应用生命周期回调中可以调用接口方法，这种调用接口的方式与页面无关，只要应用仍然在运行中，接口调用都有效。即使是这种情况，也强烈建议在确定不需要回调接口的时候，主动取消回调接口的调用。

### 常见错误代码

|代码|含义|
|:-|:-|
|200|一般性错误|
|201|用户拒绝|
|202|参数非法|
|203|服务不可用|
|204|请求超时|