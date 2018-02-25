## 容器组件
### div

概述

基本容器。

子组件

支持。

属性

支持通用属性。

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|flex-direction|column、row|row|否||
|flex-wrap|nowrap、wrap、wrap-reverse|nowrap|否||
|justify-content|flex-start、flex-end、center、space-between|flex-start|否||
|align-items|stretch、flex-start、flex-end、center|stretch|否||
|align-content|stretch、flex-start、flex-end、center、space-between、space-around|stretch|否|-|

事件

支持通用事件。

### list-item

概述

``<list>``的子组件，用来展示列表具体item，宽度默认充满list组件。

子组件

支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|type|``<string>``||是|list-item类型，同一list支持多种type的list-itemtype，相同的list-item需要确保渲染后的视图布局也完全相同，当type固定时，使用show属性代替if属性，确保视图布局不变。|

样式

支持``<div>``样式。

不支持position样式，支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|column-span|``<number>``|1|否|list-item在list中所占列数，一般用于list多列显示时。|

事件

支持通用事件。

### list

概述

列表视图容器。

子组件

仅支持``<list-item>``。

属性

支持通用属性。


|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|scrollpage|``<boolean>``|false|否|是否将list顶部页面中非list部分随list一起滑出可视区域，开启该属性会降低list渲染性能。|

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|flex-direction|column、row|column|否||
|columns|``<number>``|1|否|list显示列数|

事件

|名称|参数|描述|
|:-|:-|:-|
|scroll|``{scrollX:scrollXValue, scrollY:scrollYValue}``|列表滑动|
|scrollbottom||列表滑动到底部|
|scrolltop||列表滑动到顶部|

方法

|名称|参数|描述|
|:-|:-|:-|
|scrollTo|``{index: number(指定位置)}``|list滚动到指定item位置|

### refresh

概述

下拉刷新容器。

子组件

支持。

属性

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|offset|``<length>``|132px|否|刷新组件静止时距离顶部距离。|
|refreshing|``<boolean>``|false|否|刷新组件是否正在刷新。|

样式

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|background-color|``<color>``|white|否|刷新组件背景颜色|
|progress-color|``<color>``|black|否|刷新组件loading颜色|

事件

|名称|参数|描述|
|:-|:-|:-|
|refresh|``{refreshing: refreshingValue}``|下拉refresh组件，触发刷新操作|

### richtext

概述

富文本容器。

文本内容直接写在标签内容区，内容格式需与type相匹配，只支持静态内容，由于需要实时编译，文本内容尽量不要频繁改变，否则可能导致性能问题。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|type|html|html|否|html格式支持标准的h5标签与样式，用于静态页面的展现，复杂的富文本需求尽量采用html格式富文本，内容只支持静态内容|

样式

支持``<div>``样式，height样式设置无效。

支持通用样式。

事件

支持通用事件。

### stack

概述

基本容器，子组件排列方式为层叠排列，每个直接子组件按照先后顺序依次堆叠，覆盖前一个子组件。

子组件

支持。

属性

支持通用属性。

样式

支持``<div>``样式。

支持通用样式。

事件

支持通用事件。

### swiper

概述

滑块视图容器。

子组件

支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|index|``<number>``|0|否|当前显示的子组件索引|
|autoplay|``<boolean>``|false|否|渲染完成后，是否自动进行播放|
|interval|``<number>``|3000|否|自动播放时的时间间隔，单位毫秒|
|indicator|``<boolean>``|true|否|是否启用indicator，默认true|

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|indicator-color|``<color>``|0x7f000000|否|indicator填充颜色|
|indicator-selected-color|``<color>``|0xff33b4ff|否|indicator选中时的颜色|
|indicator-size|``<length>``|20px|否|indicator组件的直径大小|

事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``{index:currentIndex}``|当前显示的组件索引变化时触发|

方法

|名称|参数|描述|
|:-|:-|:-|
|swipeTo|``{index: number(指定位置)}``|swiper滚动到index位置|

### tab-bar

概述

``<tabs>``的子组件，用来展示tab的标签区，子组件排列方式为横向排列。

子组件

支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|mode|scrollable、fixed|fixed|否|mode为scrollable时，子组件宽度为设置宽度，当宽度之和大于tab-bar宽度，子组件可以横向滚动；mode为fixed时，子组件宽度均分tab-bar宽度，当宽度之和大于tab-bar宽度，子组件依旧均分宽度|

样式

支持通用样式。

|名称|类型|默认值|必填|
|:-|:-|:-|:-|
|height|``<length>``、``<percentage>``|100px|否|

事件

支持通用事件。

### tab-content

概述

``<tabs>``的子组件，用来展示tab的内容区，高度默认充满tabs剩余空间，子组件排列方式为横向排列。

子组件

支持。

属性

支持通用属性。

样式

支持通用样式。

事件

支持通用事件。

### tabs

概述

tab容器。

子组件

仅支持最多一个``<tab-bar>``和最多一个``<tab-content>``。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|index|``<number>``|0|否|当前active tab索引。|

样式

支持通用样式。

事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``{index: indexValue}``|tabs子组件active变化时触发。|

### popover 

概述

Popover
组件。在点击控件或者某个区域后，浮出一个气泡来引导用户，气泡内容通过子组件来定义。如果设置了遮罩层，可通过点击遮罩层的任意位置退出，否则点击气泡外任意区域退出。

子组件

仅支持``<text>``。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|target|``<string>``||是|目标元素选择器。|
|placement|``<string>``|bottomRight|否|弹出窗口位置。``{'left','right','top','bottom','topLeft','topRight','bottomLeft', 'bottomRight'}``|
|mask|``<boolean>``|false|否|是否显示遮罩背景层|

样式

支持通用样式。

事件

支持通用事件。