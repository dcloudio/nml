## 基础组件
### a

概述

超链接（默认不带下划线）。

文本内容写在标签内容区，支持转义字符""。

子组件

仅支持``<span>``。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|href|``<string>``||否|支持的格式参见页面路由中的uri参数。额外的请参考 href 参数说明。|

href 参数说明

- href还可以通过"?param1=value1"的方式添加参数，参数可以在页面中通过this.param1的方式使用
- href还可以通过"?param1=value1"的方式添加参数，参数可以在页面中通过this.param1的方式使用

示例

```html
<a href="About?param1=value1">
<a href="/about?param1=value1">
<a href="http://www.example.com/?param1=value1">
```

样式

支持``<text>``样式。

支持通用样式。

事件

支持通用事件。

### image

概述

渲染图片。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|src|``<uri>``||否|图片的uri，同时支持本地和云端路径，支持的图片格式包括静态类型（png, jpg）和动态类型（gif）。|
|alt|``<uri>``||否|加载时显示的占位图。|

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|resize-mode|cover、contain、stretch、center|cover|否|图片的缩放类型|

resize-mode 类型

|类型|描述|
|:-|:-|
|cover|保持宽高比缩小或放大，使得两边都大于或等于显示边界，居中显示。|
|contain|保持宽高比，缩小或者放大，使得图片完全显示在显示边界内，居中显示。|
|stretch|不保存宽高比，填充满显示边界。|
|center|居中，无缩放。|

事件

支持通用事件。

### progress

概述

进度条

子组件

不支持

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|percent|``<number>``|0|否|当前进度(type为circular时不生效)|
|type|horizontal、circular|horizontal|否|进度条类型，不支持动态修改|

样式

支持通用样式。

horizontal progress 底色为 ``#f0f0f0``。

circular progress 默认宽高为32px，宽高设置不一致时，circular图标为宽高的较小值。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|color|``<color>``|#33b4ff 或者 rgb(51, 180, 255)|否|进度条的颜色|
|stroke-width|``<length>``|32px|否|进度条的宽度(type为circular时不生效)|

事件

支持通用事件。

### span

概述

格式化的文本，只能作为``<text>``与``<a>``的子组件。

子组件

不支持。

属性

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|id|``<string>``||否|唯一标识|
|style|``<string>``||否|样式声明|
|class|``<string>``||否|引用样式表|
|for|``<array>``||否|根据数据列表，循环展开当前标签。|
|if|``<boolean>``||否|根据数据boolean值，添加或移除当前标签。|

样式

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|color|``<color>``|0x8a000000|否|文本颜色|
|font-size|``<number>``|30px|否|文本尺寸|
|font-style|normal、italic|normal|否||
|font-weight|normal、bold|normal|否||
|text-decoration|underline、line-through、none|none|否|-|

事件

不支持。

### text

概述

文本。

文本内容写在标签内容区，支持转义字符""。

子组件

仅支持``<a>``与``<span>``。

属性

支持通用属性。

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|lines|``<number>``|-1|否|文本行数，-1代表不限定行数|
|color|``<color>``|0x8a000000|否|文本颜色|
|font-size|``<number>``|30px|否|文本尺寸|
|font-style|normal、italic|normal|否||
|font-weight|normal、bold|normal|否||
|text-decoration|underline、line-through、none|none|否||
|text-align|left、center、right|left|否||
|line-height|``<length>``||否|文本行高|
|text-overflow|clip、ellipsis|clip|否|在设置了行数的情况下生效|

事件

支持通用事件。

### web 

#### 概述

用于显示在线的html页面。

#### 子组件

不支持。

#### 属性

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|src|``<string>``||否|需要加载的页面地址|

#### 事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|pagestart|``{url: urlString}``|开始加载网页时触发|
|pagefinish|``{url: urlString, canBack: true/false, canForward: true/false}``|网页加载完成时触发|
|titlereceive|``{title: title}``|收到网页标题时触发|
|error|``{errorMsg: errorMsg}``|网页加载出现错误时触发|

#### 方法

|名称|参数|描述|
|:-|:-|:-|
|reload||重新加载页面|
|forward||浏览历史页面中的前一个页面|
|back||浏览历史页面中的后一个页面|
|canForward|``{callback: Function}``|是否可以向前浏览|
|canBack|``{callback: Function}``|是否可以向后浏览|

canForward 回调函数返回参数

|参数|返回值类型|描述|
|:-|:-|:-|
|canForward|``<boolean>``|是否可以向前浏览|

canBack 回调函数返回参数

|参数|返回值类型|描述|
|:-|:-|:-|
|canBack|``<boolean>``|是否可以向后浏览|

示例
```html
<web src="http://www.example.com/" id="web"></web>
```

```javascript
this.$element('web').canForward({
  callback: function(e) {
    if(e) {
      // 加载历史列表中的下一个 URL
      this.$element('web').forward();
    } else {
      // TODO
    }
  }.bind(this)
});
```

### rating 

概述

星级评分。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|numStars|``<number>``|5|否|否星级总数|
|rating|``<number>``|0|否|评星数|
|stepSize|``<number>``|0.5|否|评星步长|
|indicator|``<boolean>``|false|否|是否作为一个指示器（用户不可操作）|

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|star-background|``<uri>``||否|仅支持本地路径|
|star-foreground|``<uri>``||否|仅支持本地路径|
|star-secondary|``<uri>``||否|仅支持本地路径|

事件

支持通用事件，不支持点击事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``{rating: current rating}``|评星数发生改变时触发|
