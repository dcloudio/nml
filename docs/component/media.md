## 媒体组件
### audio 

概述

音频播放器。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|src|``<uri>``||否|视频播放内容的uri|
|autoplay|``<boolean>``|false|否|渲染后是否自动播放|
|loop|``<boolean>``|false|否|音频结束时是否重新开发播放|
|controls|``<boolean>``|false|否|是否显示默认控件|

样式

支持通用样式。

事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|prepared||音频连接成功时触发|
|start||开始播放时触发|
|pause||暂停时触发|
|finish||播放结束时触发|
|error||播放失败时触发|
|seeking|``{currenttime: value(秒)}``|播放进度条滑动时触发|
|seeked|``{currenttime: value(秒)}``|播放进度条滑动放开时触发|
|timeupdate|``{currenttime: value(秒)}``|播放进度变化时触发，触发频率4HZ|

方法

|名称|参数|描述|
|:-|:-|:-|
|start||开始播放音频|
|pause||暂停播放音频|
|setCurrentTime|``{currenttime: value(秒)}``|跳转到指定位置，单位为s|

### video

概述

视频播放器。

子组件

不支持。

属性

支持通用属性。

|名称|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|src|``<uri>``||否|视频播放内容的uri|
|autoplay|``<boolean>``|false|否|渲染后是否自动播放|
|poster|``<uri>``||否|视频预览海报|

样式

支持通用样式。

事件

支持通用事件。

|名称|参数|描述|
|:-|:-|:-|
|prepared||视频连接成功时触发|
|start||开始播放时触发|
|pause||暂停时触发|
|finish||播放结束时触发|
|error||播放失败时触发|
|seeking|``{currenttime: value(秒)}``|播放进度条滑动时触发|
|seeked|``{currenttime: value(秒)}``|播放进度条滑动放开时触发|
|timeupdate|``{currenttime: value(秒)}``|播放进度变化时触发，触发频率4HZ|
|fullscreenchange|``{fullscreen: fullscreenValue}``|视频进入和退出全屏时触发|

方法

|名称|参数|描述|
|:-|:-|:-|
|start||开始播放视频|
|pause||暂停播放视频|
|setCurrentTime|``{currenttime: value(秒)}``|指定视频播放位置|
|requestFullscreen||视频进入全屏|
|exitFullscreen||视频退出全屏|
