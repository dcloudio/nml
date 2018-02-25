## 通用
### 通用事件

|名称|参数|描述|
|:-|:-|:-|
|click||组件被点击时触发|
|longpress||组件被长按时触发|
|focus||组件获得焦点时触发|
|blur||组件失去焦点时触发|
|appear||组件出现时触发|
|disappear||组件消失时触发|
|swipe|direction:String|swipe 事件在用户在某个元素上快速滑动超过 30px 时被触发。|

### 通用属性

#### 常规属性

|名称|类型|默认值|描述|
|:-|:-|:-|:-|
|id|``<string>``||唯一标识|
|style|``<string>``||样式声明|
|class|``<string>``||引用样式表|
|disabled|``<string>``|false|表明当前组件是否可用|

#### 渲染属性

|名称|类型|默认值|描述|
|:-|:-|:-|:-|
|for|``<array>``||根据数据列表，循环展开当前标签|
|if|``<boolean>``||根据数据boolean值，添加或移除当前标签|
|show|``<boolean>``||根据数据boolean值，显示或隐藏当前标签，相当于控制{ display: flex | none }|

渲染属性工作方式详见“template模板”。

属性与样式不能混用，不能在属性字段中进行样式设置。

### 通用样式

|名称|类型|默认值|描述|
|:-|:-|:-|:-|
|width|``<length>``、``<percentage>``||未设置时使用组件自身内容需要的宽度。|
|height|``<length>``、``<percentage>``||未设置时使用组件自身内容需要的高度。|
|padding|``<length>``|0|简写属性，在一个声明中设置所有的内边距属性，该属性可以有1到4个值。|
|padding-[left/top/right/bottom]|``<length>``|0||
|margin|``<length>``|0|简写属性，在一个声明中设置所有的外边距属性，该属性可以有1到4个值。|
|margin-[left/top/right/bottom]|``<length>``|0||
|border||0|简写属性，在一个声明中设置所有的边框属性，可以按顺序设置属性width style color，不设置的值为默认值。|
|border-style|dotted、dashed、solid|solid|暂时仅支持1个值，为元素的所有边框设置样式。|
|border-width|``<length>``|0|简写属性，在一个声明中设置元素的所有边框宽度，或者单独为各边边框设置宽度。|
|border-[left/top/right/bottom]-width|``<length>``|0||
|border-color|``<color>``|black|简写属性，在一个声明中设置元素的所有边框颜色，或者单独为各边边框设置颜色。|
|border-[left/top/right/bottom]-color|``<color>``|black||
|border-radius|``<length>``|0|圆角时只使用border-width，border-[left/top/right/bottom]-width，无效圆角时只使用border-color，border-[left/top/right/bottom]-color无效|
|border-[top/bottom]-[left/right]-radius|``<length>``|0||
|background|``<linear-gradient>``||支持渐变样式，暂时不能与background-color、background-image同时使用。|
|background-color|``<color>``|||
|background-image|``<uri>``||暂时不支持与background-color，border-color同时使用；不支持网络图片资源，请使用本地图片资源。|
|background-size|contain、cover、auto、``<length>``、``<percentage>``||设置背景图片大小|
|opacity|``<number>``|0xff||
|display|flex、none|flex||
|visibility|visible、hidden|visible||
|flex|``<number>``||父容器为``<div>``、``<list-item>``、``<tabs>``时生效|
|flex-grow|``<number>``|0|父容器为``<div>``、``<list-item>``时生效|
|flex-shrink|``<number>``|1|父容器为``<div>``、``<list-item>``时生效|
|flex-basis|``<number>``|-1|父容器为``<div>``、``<list-item>``时生效|
|position|none、fixed|none||
|[left/top/right/bottom]|``<number>``|-|-|

### 动画样式

|名称|类型|默认值|描述|
|:-|:-|:-|:-|
|transform-origin|``<position>``|0px 0px|变换原点位置，单位目前仅支持px，格式为：50px 100px。|
|transform|``<string>``||详见 transform 操作说明|
|animation-name|``<string>``||与@keyframes的name相呼应|
|animation-delay|``<time>``|0|目前支持的单位为[s(秒)\ms(毫秒)]。|
|animation-duration|``<time>``|0|目前支持的单位为[s(秒)\ms(毫秒)]。|
|animation-iteration-count|``<integer>``|1||
|animation-timing-function|linear、ease、ease-in、ease-out、ease-in-out|linear||
|animation-fill-mode|none、forwards|none|-|

transform 操作说明

|名称|类型|
|:-|:-|
|translateX|``<length>``|
|translateY|``<length>``|
|scaleX|``<multiple>``|
|scaleY|``<multiple>``|
|rotate|``<deg>``、``<rad>``|
|rotateX|``<deg>``、``<rad>``|
|rotateY|``<deg>``、``<rad>``|

@keyframes 属性说明

|名称|类型|默认值|描述|
|:-|:-|:-|:-|
|background-color|``<color>``|||
|opacity|``<number>``|||
|width/height|``<length>``||暂不支持百分比|
|transform|``<string>``||-|

暂时不支持起始值(0%)或终止值(100%)缺省的情况，通过from和to显示指定起始和结束。示例：

```css
@keyframes Go {
  from {
    background-color: #f76160;
  }
  to {
    background-color: #09ba07;
  }
}

```

### 渐变样式
渐变 (gradients) 可以在两个或多个指定的颜色之间显示平稳的过渡，用法与CSS渐变一致。
当前框架支持以下渐变效果：
- 线性渐变 (linear-gradient)
- 重复线性渐变 (repeating-linear-gradient)

#### 线性渐变 / 重复线性渐变
创建一个线性渐变，需要定义两类数据：1) 过渡方向；2) 过渡颜色，因此需要指定至少两种颜色。
1. 过渡方向：通过direction或者angle两种形式指定
2. 过渡颜色：支持后面三种方式：red、#FF0000、rgb(255, 0, 0)、rgba(255, 0, 0, 1)


- direction：方向渐变
  - background: linear-gradient(direction, color-stop1, color-stop2, ...)
  - background: repeating-linear-gradient(direction, color-stop1, color-stop2, ...)
- angle：角度渐变
  - background: linear-gradient(angle, color-stop1, color-stop2)
  - background: repeating-linear-gradient(angle, color-stop1, color-stop2)


参数

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|direction|to ``<side-or-corner>``  ``<side-or-corner>`` = [left/right]、[top/bottom]|to bottom (从上到下渐变)|否|例如：to right (从左向右渐变)；例如：to bottom right (从左上角到右下角)。|

示例

```css
#gradient {
  height: 100px;
  width: 200px;
}


/* 从顶部开始渐变。起点是红色，慢慢过渡到蓝色 */
background: linear-gradient(red, blue);

/* 45度夹角，从红色渐变到蓝色  */
background: linear-gradient(45deg, rgb(255, 0, 0), rgb(0, 0, 255));

/* 从左向右渐变，在距离左边90px和距离左边120px (200*0.6) 之间30px宽度形成渐变*/
background: linear-gradient(to right, rgb(255,0,0) 90px, rgb(0, 0, 255) 60%);

/* 从左向右渐变，重复渐变区域10px（20-10）透明度0.5 */
background: repeating-linear-gradient(to right, rgba(255, 0, 0, .5) 10px,rgba(0, 0, 255, .5) 20px);
```
