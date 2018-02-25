## 表单组件
### input

概述

提供可交互的界面，接收用户的输入，默认为单行。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|type|button、checkbox、radio、text、email、date、time、number、password|text|否|不支持动态修改|
|checked|``<boolean>``|false|否|当前组件的checked状态，可触发checked伪类，type为checkbox时生效。|
|name|``<string>``||否|input组件名称|
|value|``<string>``||否|input组件的值|

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|color|``<color>``|0xde000000|否|文本颜色|
|font-size|``<number>``|37.5px|否|文本尺寸|
|placeholder|``<string>``|37.5px|否|提示文本的内容，type为text、email、date、time时生效|
|placeholder-color|``<color>``|0x61000000|否|提示文本的颜色，type为text、email、date、time时生效|
|width|``<length>``、``<percentage>``||否|type为button时，默认值为128px|
|height|``<length>``、``<percentage>``||否|type为button时，默认值为128px|

事件

支持通用事件。

支持change事件，input组件的值、checked状态发生变化时触发。

|参数|text、email、date、time、number|checkbox|radio|备注|
|name||✔|✔||
|value|✔|✔|✔||
|checked||✔||-|

方法

|名称|参数|描述|
|:-|:-|:-|
|focus|``{focus:true|false}``，focus 不传默认为 true。|使组件获得或者失去焦点，可触发focus伪类，type为text、email、date、time、number、password时，可弹出或收起输入法。|

### label 

概述

为input、textarea组件定义标注。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|target|``<string>``||否|目标input组件id|

样式

支持通用样式。

支持text样式。

事件

无。

### option 

概述

``<select>``的子组件，用来展示下拉选择具体项目。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|selected|``<boolean>``||否|选择项是否为下拉列表的默认项|
|value|``<string>``||是|选择项的值|

样式

支持通用样式。

事件

支持通用事件。

### picker

概述

滚动选择器，目前支持三种选择器：普通选择器，日期选择器，时间选择器。默认为普通选择器。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|type|text、date、time||是|不支持动态修改。|
|start|``<time>``|1970-1-1|否|type为date时生效，起始时间，格式为yyyy-MM-dd。|
|end|``<time>``|2100-12-31|否|type为date时生效，结束时间，格式为yyyy-MM-dd。|
|range|``<array>``||否|type为text时生效，选择器的取值范围。|
|selected|``<string>``|当前时间或0|否|选择器的默认取值。type为date时，格式为yyyy-MM-dd；type为time时，格式为hh:mm；type为text时，取值为range的索引。|
|value|``<string>``||否|选择器的值|

样式

支持通用样式。

事件

不支持click事件，支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``date: {year:year,month:month,day:day}; time: {hour:hour,minute:minute};text: {newValue:newValue,newSelected:newSelected}``|滚动选择器选择值后确定时触发（newSelected为索引）|

方法

|名称|参数|描述|
|:-|:-|:-|
|show||显示picker|

### select 

概述

下拉选择按钮，点击会弹出一个包含所有可选值的下拉菜单，从该菜单中可以选择一个新值。

子组件

仅支持``<option>``。

属性。

支持通用属性。

样式

支持通用样式。

事件

不支持click事件，支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``{newValue:newValue}``|下拉选择器选择值后触发|

### slider

概述

滑动选择器。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|min|``<number>``|0|否||
|max|``<number>``|100|否||
|step|``<number>``|1|否||
|value|``<number>``|0|否|-|

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|color|``<color>``|0xfff0f0f0|否|背景条颜色|
|selected-color|``<color>``|0xff009688|否|已选择颜色|
|padding-[left/right]|``<length>``|32px|否|左右边距|

事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``{progress:progressValue}``|完成一次拖动后触发的事件|

### switch

概述

开关选择。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|checked|``<boolean>``|false|否|可触发checked伪类|

样式

支持通用样式。

事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``{checked:checkedValue}``|checked状态改变时触发|

### textarea

概述

提供可交互的界面，接收用户的输入，默认为多行。

子组件

不支持。

属性

支持``<text>``属性

支持通用属性。

样式

支持通用样式。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|color|``<color>``|0xde000000|否|文本颜色|
|font-size|``<number>``|37.5px|否|文本尺寸|
|placeholder|``<string>``||否|提示文本的内容|
|placeholder-color|``<color>``|0x61000000|否|提示文本的颜色|

事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|change|``{text:newText}``|输入内容发生变化时触发|

方法

|名称|参数|描述|
|:-|:-|:-|
|focus|``{focus:true、false}``，focus不传默认为true。|使组件获得或者失去焦点，可触发focus伪类，可弹出或收起输入法。|
