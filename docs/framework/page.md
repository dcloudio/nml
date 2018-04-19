## 页面与组件
### template 模板
类似HTML的标签语言，结合基础组件、自定义组件、事件，构建出页面的结构。

模板中只能有1个根节点，如：div；请不要在 ``<template>`` 下存在多个根节点，也不要使用 ``block`` 作为根节点。

**数据绑定**
```html
<template>
  <text>{\{message}}</text>
</template>

<script>
  module.exports = {
    data: {
      message: 'Hello World!'
    }
  }
</script>
```
**事件绑定**
```html
<template>
  <div>
    <!-- 正常格式 -->
    <div onclick="press"></div>
    <!-- 缩写 -->
    <div @click="press"></div>
  </div>
</template>

<script>
  module.exports = {
    press: function(e) {
      this.title = 'Hello'
    }
  }
</script>
```
事件回调支持的写法（其中花括号可以省略）：
- "fn"：fn为事件回调函数名（在 ``<script>`` 中有对应的函数实现）。
- "fn(a,b)"：函数参数例如a，b可以是常量，或者是在 ``<script>`` 的data中定义的变量（前面不用写this.）。
- "a+b"：表达式，其中a,b数据类型与上面相同。

回调函数被调用时，会在参数列表末尾自动添加一个 ev 参数，通过 evt 参数访问回调事件相关上下文数据（数据内容具体参看组件回调事件说明），例如点击事件的点击位置 x，y。

**列表渲染**
```html
<template>
  <div class="list-container">
    <text for="{\{list}}" tid="id">
      <text>{\{$idx}}<text>
      <text>{\{$item.id}}<text>
    </text>
  </div>
</template>

<script>
  module.exports = {
    data: {
      list: [{
        id: 1
      }, {
        id: 2
      }]
    }
  }
</script>
```

标签中的 tid 用于指定作为定义数组元素唯一 ID 的字段名，如果没有指定，则默认采用数组索引（$idx）作为唯一 ID；tid 的作用是为了元素节点重用，优化 for 循环的重绘效率。

例如 ``tid="id"`` 表示 ``$item.id`` 作为数组元素的唯一 ID。

for 循环支持的写法（其中花括号可以省略）：
- for="list"：list 为数组对象，默认元素变量为 $item。
- for="value in list"：value 为自定义的元素变量，默认元素索引为 $idx。
- for="(index,value) in list"：index 为元素索引，value 为元素变量。
- tid 指定的数据属性必须存在，否则可能引起运行异常。
- tid 指定的数据属性要保证唯一，如果发生重复，可能会造成性能问题。

**条件渲染**

分为2种：if/elif/else 和 show。它们的区别在于：

- if 为 false 时，组件会从 VDOM 中移除，而 show 仅仅是渲染时不可见，组件依然存在于 VDOM 中。
- if/elif/else节点必须是相邻的兄弟节点，否则无法通过编译。

```html
<template>
  <div>
    <text if="{\{show}}"> Hello-1 </text>
    <text elif="{\{display}}"> Hello-2 </text>
    <text else> Hello-3 </text>
  </div>
</template>

<script>
  module.exports = {
    data: {
      show: false
    }
  }
</script>
```

show 等同于 ``display=none``，目前只用于系统原生组件，对自定义组件不生效。自定义组件可以通过 props 传入参数，在自己内部使用 show 来控制是否可见。
```html
<template>
  <text show="{\{visible}}"> Hello </text>
</template>

<script>
  module.exports = {
    data: {
      visible: false
    }
  }
</script>
```
**逻辑控制块**

可以使用 <block> 实现更为灵活的循环/条件渲染；
注意 <block> 目前只支持 for 和 if/elif/else 属性，如果没有指定任何属性，<block> 则在构建时被当作"透明"节点对待，其子节点被添加到 <block> 的父节点上。
```html
<template>
  <list>
    <block for="cities">
      <list-item type="city">
        <text>{\{$item.name}}</text>
      </list-item>
      <block for="$item.spots" if="false">
        <list-item type="spot">
          <text>{\{$item.address}}</text>
        </list-item>
      </block>
    </block>
  </list>
</template>

<script>
  module.exports = {
    data: {
      cities: [{
          name: "beijing",
          spots: [{
            name: "XXX",
            address: "XXX"
          }, {
            name: "XXX",
            address: "XXX"
          }]
        },
        {
          name: "shanghai",
          spots: [{
            name: "XXX",
            address: "XXX"
          }, {
            name: "XXX",
            address: "XXX"
          }]
        }
      ]
    }
  }
</script>
```

**引入自定义组件**
```html
<import name='comp' src='./comp'></import>
<template>
  <div>
    <comp prop1='xxxx' onevent1="bindParentVmMethod1" @event-type1="bindParentVmMethod1"></comp>
  </div>
</template>
```

如果没有设置 name 属性，则默认采用 src 的文件名作为组件名

src 属性指定组件 nml 文件位置，可以省略 .nml 后缀。

- 组件名不区分大小写，默认统一采用小写。
- 通过 (on|@)event1 语法绑定自定义子组件的 event1 事件，触发事件 ``childVm.$emit('event1', { params: '传递参数' })`` 时执行父组件的方法：bindParentVmMethod1。
- 标签上声明事件名称采用-连接，不要使用驼峰式，来做响应与方法的关联，即：使用 event-type1 表示绑定 eventType1 事件。

**插入自定义组件**

组件中通过标签来定义子组件插入位置，例如：

组件 com-a 的模板定义为：
```html
<template>
  <div>
    <text>header</text>
    <slot></slot>
    <text>footer</text>
    <div>
</template>
```
在页面使用组件 comA，定义如下：
```html
<import name="comp-a" src="./comp-a"></import>
<template>
  <com-a>
    <text>body</text>
  </com-a>
</template>
```
则在页面渲染时，组件 comA 变为：
```html
<div>
  <text>header</text>
  <text>body</text>
  <text>footer</text>
</div>
```
### Style样式

用于描述 template 模板的组件样式，决定组件应该如何显示。

样式布局采用CSS Flexbox（弹性盒）样式，针对部分原生组件，对 CSS 进行了少量的扩充以及修改。

为了解决屏幕适配问题，所有与大小相关的样式（例如width、font-size）均以基准宽度（默认750px）为基础，根据实际屏幕宽度进行缩放，例如width:100px，在1500px宽度屏幕上，width实际上为200px。

文件导入

支持2种导入外部文件的方式：
```html
<!-- 导入外部文件, 代替style内部样式 -->
<style src="./style.css"></style>
```
```html
<!-- 合并外部文件 -->
<style>
@import './style.css';
a{
}
</style>
```

**模板内部样式**

支持使用style、class属性来控制组件的样式。
```html
<!-- 内联inline -->
<div style="color:red; margin: 10px;" />
<!-- class声明 -->
<div class="normal append" />*
```

**选择器**

支持的选择器如下表所示。

|选择器|样例|样例描述|
|:-|:-|:-|
|.class|.intro|选择所有拥有 class="intro" 的组件。|
|#id|#firstname|选择拥有 id="firstname" 的组件。|
|tag|div|选择所有 div 组件。|
|,|.a, .b|选择所有class=".a" 以及class=".b"的组件。|

```html
<style>
// 单个选择器
text {
}
.class-abc {
}
#idAbc {
}

// 同一样式适应多个选择器
.font-text,.font-comma {
}
</style>
```

选择器声明的变化可能会导致元素重新绘制。为了减少选择器变化引起的DOM更新数量，当前只支持：CSS声明的多个选择器中最后一个规则的变更对DOM的更新。

```html
<template>
  <div class="{\{docBody}}">
    <text class="{\{rowDesc}}" value="描述内容"></text>
  </div>
</template>

<style>
  .row-desc1 {
    color: #ff0000;
  }
  
  .row-desc2 {
    color: #0000ff;
  }
  
  .row-desc1 {
    color: #00ff00;
  }
</style>

<script>
  export default {
    data: {
      rowDesc: "row-desc1",
      docBody: "doc-body"
    }
  }
</script>
```

以上的代码示例，当我们把 rowDesc 变量从 row-desc1 变为 row-desc2 时是通知 Native 更新节点样式，但是如果把 docBody 变量从 doc-body 变为 doc-body2，是不会通知 DOM 更新的。

因为 doc-body 不是最后一个选择器，非末尾的选择器变更有可能影响很多 DOM 元素，从而影响到渲染性能。

**选择器优先级**

当前样式的选择器的优先级计算保持与浏览器一致，是浏览器 CSS 渲染的一个子集（仅支持：inline，id，class，tag，后代，直接后代）

选择器优先级规则如下（假设多条 CSS 声明匹配到同一个元素 div），应用在该元素上的 CSS 声明总体优先级是：``inline > #id > .class > tag``，这四大类匹配到该元素的多个 CSS 声明。

**样式预编译**

目前支持 less，sass 的预编译。
```html
<!--导入外部文件, 代替style内部样式-->
<style lang="less" src="./lessFile.less">
</style>
<!--合并外部文件-->
<style lang="less">
@import './lessFile.less';

.page-less {
  #testTag {
    .less-font-text,
    .less-font-comma {
      font-size: 60px;
    }
  }
}
</style>
```
**伪类**

任何组件中，如果某个属性是boolean类型且默认值为false时，均可通过该属性名字来声明伪类，当属性变为true时伪类生效，例如所有组件的disabled属性、input组件的checked属性等。

另外部分组件会有其他形式的伪类支持，比如input组件可以通过主动调用focus方法，或者用户操作获得焦点，使得focus伪类生效，详情请参考各组件内部说明。

伪类写法举例：
```html
<template>
  <input type="button" class="btn" disabled="{\{btndisabled}}">Click</input>
</template>
<style>
.btn {
  width: 360px;
  height: 180px;
  background-color: red;
}

.btn:disabled {
  background-color: green;
}
</style>
```

当组件的disabled属性变为true时，disabled伪类的样式生效，叠加到原有样式上，例子中background-color会从红色变成绿色。

### script脚本

用来定义页面数据和实现生命周期接口。

#### 语法

支持ES6语法

**代码引用**

NJS 代码引用推荐使用 ``import`` 来导入，例如：
```javascript
import utils from '../Common/utils.njs'
```

#### 对象

**自定义数据**

|属性|类型|描述|
|:-|:-|:-|
|data|Object/Function|页面的数据模型，能够转换为JSON对象；属性名不能以$或_开头，不要使用for, if, show, tid等保留字。如果是函数，返回结果必须是对象，在页面初始时回执行函数获取结果作为data的值。|
|props|Array|用于定义组件的公开属性列表（外部可见），可以通过 ``<tag xxxx='value'>`` 方式传递给到组件内部；属性名必须全部小写， 不能以$或_开头，不要使用for, if, show, tid等保留字。目前公开属性的数据类型不支持function。|

**公共对象**

|属性|类型|描述|
|:-|:-|:-|
|$app|Object|应用对象。|
|$page|Object|页面对象。|
|$valid|Boolean|页面对象是否有效。|
|$visible|Boolean|页面是否处于用户可见状态。|

**应用对象**

可通过 ``$app`` 访问。

|属性|类型|描述|
|:-|:-|:-|
|$def|Object|使用this.$app.$def获取在app.ux中暴露的对象。|
|$data|Object|使用this.$app.$data获取在manifest.json的config.data中声明的全局数据。|

**页面对象**

可通过 ``$page`` 访问。

|属性|类型|描述|
|:-|:-|:-|
|action|String|获取打开当前页面的action。仅在当前页面是通过filter匹配的方式打开时有效，否则为undefined。参见"manifest文件"。|
|uri|String|获取打开当前页面的uri。仅在当前页面是通过filter匹配的方式打开时有效，否则为undefined。参见"manifest文件"。|

#### 方法

**数据方法**

|属性|类型|参数|描述|
|:-|:-|:-|:-|
|$set|Function|key: String value: Any|添加数据属性，必须在 onInit 函数中使用，否则 ``<template>`` 中数据绑定无法生效用法 ``this.$set('key', alue)`` ``this.$vm('id').$set('key',value)``|
|$delete|Function|key: String|删除数据属性，如果在 onInit 函数中使用，会导致 ``<template>`` 中相应的数据绑定无法生效用法 ``this.$delete('key')`` ``this.$vm('id').$delete('key')``|

**公共方法**

|属性|类型|参数|描述|
|:-|:-|:-|:-|
|$element|Function|id: String 组件id|获取指定id的组件dom对象，如果没有指定id，则返回根组件dom对象用法 ``<template> <div id='xxx'></div></template>``。``this.$element('xxx')`` 获取id为xxx的div组件实例对象。``this.$element()`` 获取根组件实例对象。|
|$rootElement|Function|无|获取根组件dom对象用法 ``this.$rootElement()``，获取根组件实例对象，等同于 ``this.$element()``|
|$vm|Function|id: String 组件id|获取指定 id 的自定义组件的 ViewModel 用法 ``this.$vm('xxx')``，获取 id 为xxx的 div 组件 ViewModel。|
|$forceUpdate|Function|无|强制页面刷新|
|$child|Function|id: String 组件id|获取指定id的自定义组件的ViewModel用法：this.$child('xxx') 获取id为xxx的div组件ViewModel|

**事件方法**

|属性|类型|参数|描述|
|:-|:-|:-|:-|
|$on|Function|type: String 事件名，handler: Function 事件句柄函数|添加事件处理句柄用法 ``this.$on('xxxx', this.fn)`` ，fn 是在 ``<script>`` 中定义的函数。|
|$off|Function|type: String 事件名，handler: Function 事件句柄函数|删除事件处理句柄用法 ``this.$off('xxxx', this.fn)`` ，``this.$off('xxx')``删除指定事件的所有处理句柄。|
|$dispatch|Function|type: String 事件名|向上层组件发送事件通知用法 ``this.$dispatch('xxx')`` 正常情况下，会一直向上传递事件（冒泡），如果要停止冒泡，在事件句柄函数中调用 ``evt.stop()`` 即可。|
|$broadcast|Function|type: String 事件名|向子组件发送事件通知用法 ``this.$broadcast('xxx')`` 正常情况下，会一直向下传递事件，如果要停止传递，在事件句柄函数中调用 ``evt.stop()`` 即可。|
|$emit|Function|type: String 事件名，data: Object 事件参数|触发事件，对应的句柄函数被调用用法 ``this.$emit('xxx')`` ，``this.$emit('xxx', {a:1})``传递的事件参数可在事件回调函数中，通过 ``evt.detail`` 来访问，例如 ``evt.detail.a``。|
|$emitElement|Function|type: String 事件名，data: Object 事件参数，id: String 组件id (默认为-1 代表根元素)|触发组件事件, 对应的句柄函数被调用用法 ``this.$emitElement('xxx', null, 'id')``， ``this.$emitElement('xxx', {a:1})`` 传递的事件参数可在事件回调函数中，通过 ``evt.detail`` 来访问，例如 ``evt.detail.a``。|
|$watch|Function|data: String 属性名, 支持'a.b.c'格式，不支持数组索引，handler: String 事件句柄函数名, 函数的第一个参数为新的属性值，第二个参数为旧的属性值|动态添加属性/事件绑定，属性必须在data中定义，handler函数必须在 ``<script>`` 定义；当属性值发生变化时事件才被触发用法 ``this.$watch('a','handler')``|

**页面方法**

可通过 $page 访问。

|属性|类型|参数|描述|
|:-|:-|:-|:-|
|setTitleBar|Function|text: String 标题栏文字，textColor: String 文字颜色，backgroundColor: String 背景颜色，backgroundOpacity: Number 透明度，menu: Boolean 是否显示菜单按钮|设置当前页面的标题栏用法 ``this.$page.setTitleBar({text:'Hello', textColor:'#FF0000', backgroundColor:'#FFFFFF',backgroundOpacity :0.5,menu: false})``|


#### 生命周期接口

**页面生命周期**

|属性|类型|参数|返回值|描述|触发时机|
|:-|:-|:-|:-|:-|:-|
|onInit|Function|无|无|监听页面初始化|当页面完成初始化时调用，只触发一次。|
|onReady|Function|无|无|监听页面创建完成|当页面完成创建可以显示时触发，只触发一次。|
|onShow|Function|无|无|监听页面显示|当进入页面时触发|
|onHide|Function|无|无|监听页面隐藏|当页面跳转离开时触发|
|onDestroy|Function|无|无|监听页面退出|当页面跳转离开（不进入导航栈）时触发。|
|onBackPress|Function|无|Boolean|监听返回按钮动作|当用户点击返回按钮时触发。返回true表示页面自己处理返回逻辑，返回false表示使用默认的返回逻辑，不返回值会作为false处理。|
|onMenuPress |Function|无|无|监听菜单按钮动作|当用户点击 ActionBar 右边菜单按钮时触发|
|onOrientationChange |Function|无|无|监听设备屏幕旋转|用户垂直或水平旋转移动设备时被触发，回调函数传入对象 ScreenOrientation 来描述当前屏幕转向信息。|

``ScreenOrientation.type`` 描述当前转向类型

- portraitprimary
- portraitsecondary
- landscapeprimary
- landscapesecondary

``ScreenOrientation.angle`` 描述当前转向角度

页面的生命周期接口的调用顺序：

- 打开页面A：onInit() -> onReady() -> onShow()
- 在页面A打开页面B：onHide()
- 从页面B返回页面A：onShow()
- A页面返回：onBackPress() -> onHide() -> onDestroy()

**应用生命周期**

|属性|类型|参数|返回值|描述|触发时机|
|:-|:-|:-|:-|:-|:-|
|onCreate|Function|无|无|监听应用创建|当应用创建时调用|
|onDestroy|Function|无|无|监听应用销毁|当应用销毁时触发|
