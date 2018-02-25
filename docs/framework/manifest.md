## manifest文件
manifest.json 文件中包含了应用描述、接口声明、页面路由信息。

|属性|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|package|String|-|是|应用包名，确认与原生应用的包名不一致，推荐采用 ``com.company.module`` 的格式，如：com.example.demo。|
|name|String|-|是|应用名称，6个汉字以内，与应用商店保存的名称一致，用于在桌面图标、弹窗等处显示应用名称。|
|icon|String|-|是|应用图标，提供192x192大小的即可。|
|versionName|String|-|否|应用版本名称，如："1.0"。|
|versionCode|Integer|-|是|应用版本号，从1自增，推荐每次重新上传包时 versionCode+1。|
|minPlatformVersion|Integer|1000|否|支持的最小平台版本号，原理同 Android API Level，兼容性检查，避免上线后在低版本平台运行并导致不兼容。|
|features|Array|-|否|接口列表，绝大部分接口都需要在这里声明，否则不能调用，详见每个接口的文档说明。|
|config|Object|-|是|系统配置信息，详见"config"。|
|router|Object|-|是|路由信息，详见"router"。|
|display|Object|-|否|UI显示相关配置，详见"display"。|
### config
用于定义系统配置和全局数据。

|属性|类型|默认值|描述|
|:-|:-|:-|:-|
|logLevel|String|log|打印日志等级，分为 off、error、warn、info、log、debug。|
|designWidth|Integer|750|页面设计基准宽度，根据实际设备宽度来缩放元素大小。|
|data|Object|-|全局数据对象，属性名不能以$或_开头，在页面中可通过 this 进行访问；如果全局数据属性与页面中 data 属性重名，则页面初始化时，全局数据会覆盖页面中对应的属性值。|
### router

用于定义页面的组成和相关配置信息，如果页面没有配置路由信息，则在编译打包时跳过。
|属性|类型|默认值|描述|
|:-|:-|:-|:-|
|entry|String|-|首页名称。|
|pages|Object|-|页面配置列表，key 值为页面名称（对应页面目录名，例如Hello对应'Hello'目录），value 为页面详细配置 page，详见"router.page"。|

#### router.page
用于定义单个页面路由信息。

|属性|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|component|String|-|是|页面对应的组件名，与 nml 文件名保持一致，例如'hello'对应'hello.nml'。|
|path|String|/|否|页面路径，例如"/user",不填则默认为/<页面名称>。path 必须唯一，不能和其他 page 的 path 相同。下面 page 的 path 因为缺失，会被设置为``"/Index":"Index": {"component": "index"}``。|
|filter|Object|-|否|声明页面可以处理某种请求，作用返回限定在应用内。|

#### router.page.filter

声明页面可以处理某种请求，页面可以从 $page 获取打开页面的参数，参见 script 脚本。filter 的结构如下：
```json
"filter": {
  "<action>": {
    "uri": "<pattern>"
  }
}
```

|属性|类型|默认值|必填|描述|
|:-|:-|:-|:-|:-|
|action|String|-|是|请求的动作，目前仅支持 view 这一种|
|uri|Pattern|-|是|请求的数据的匹配规则。必须是正则表达式。如 ``https?://.*`` 可以匹配所有 http 和 https 类型的网址。|

可以处理所有 http 和 https 请求的 filter 定义如下：
```json
"filter": {
  "view": {
    "uri": "https?://.*"
  }
}
```

### display

用于定义与 UI 显示相关的配置。

|属性|类型|默认值|描述|
|:-|:-|:-|:-|
|backgroundColor|String|#ffffff|窗口背景颜色。|
|fullScreen|Boolean|false|是否是全屏模式，全屏模式下状态栏也会被覆盖，默认不会同时作用于 titleBar ，titleBar 需要继续通过 titleBar 控制。|
|titleBar|Boolean|true|是否显示 titleBar。|
|titleBarBackgroundColor|String|-|标题栏背景色。|
|titleBarTextColor|String|-|标题栏文字颜色。|
|titleBarText|String|-|标题栏文字（也可通过页面跳转传递参数（titleBarText）设置）。|
|menu|Boolean|false|是否显示标题栏右上角菜单按钮。|
|pages|Object|-|各个页面的显示样式，key 为页面名（与路由中的页面名保持一致），value 为窗口显示样式，页面样式覆盖 default 样式。|

### 示例
```json
{
  "package": "com.company.unit",
  "name": "appName",
  "icon": "/Common/icon.png",
  "versionName": "1.0",
  "versionCode": 1,
  "minPlatformVersion": 1000,
  "features": [
    {
      "name": "system.network"
    }
  ],
  "permissions": [
    {
      "origin": "*"
    }
  ],
  "config": {
    "logLevel": "off"
  },
  "router": {
    "entry": "Hello",
    "pages": {
      "Hello": {
        "component": "hello",
        "path": "/",
        "filter": {
          "view": {
            "uri": "https?://.*"
          }
        }
      }
    }
  },
  "display": {
    "backgroundColor": "#ffffff",
    "fullScreen": false,
    "titleBar": true,
    "titleBarBackgroundColor": "#000000",
    "titleBarTextColor": "#fffff",
    "pages": {
      "Hello": {
        "backgroundColor": "#eeeeee",
        "fullScreen": true,
        "titleBarBackgroundColor": "#0000ff",
        "titleBarText": "Hello"
      }
    }
  }
}
```