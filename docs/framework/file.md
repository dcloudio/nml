## 文件组织
### 应用资源
一个应用包含：描述项目配置信息的 manifest 文件，放置项目公共资源脚本的 app.njs ，多个描述页面/自定义组件的 nml 文件，典型示例如下：  
应用根目录

- manifest.json
- app.njs
- Page1
  - page1.nml
- Page2
  - page2.nml
- Common
  - ComponentA.nml
  - ComponentB.nml
  - xxx.png

其中 Common 目录下为公用的资源文件和组件文件，每个页面目录下存放各自页面私有的资源文件和组件文件，如：图片，NSS，NJS 等。

### 文件存储

在应用平台中是按分区来存储文件的，目前支持以下分区：

- Cache：一般用于存储缓存文件，比如通过 fetch 接口下载的文件会存储在该分区中，该分区中的文件可能因存储空间不够被系统删除。

- Files：一般用于存储比较小的永久文件，该分区中的文件由应用自己管理。

- Mass：一般用于存储比较大的文件，但该分区并不保证一直可用。

- Temp：表示从外部映射过来的临时文件，出于安全性考虑，临时文件是只读的，并且只能通过调用特定的 API 获取，比如 ``uni.media.pickVideo`` 方法。另外临时文件的访问是临时的，应用重启后无法访问到临时文件，需要通过特定 API 重新获取。

另外应用资源也作为一个特殊的只读分区进行处理。

### URI

URI 用于标识应用资源和文件，组件和接口通过 URI 来访问应用资源和文件。

|资源类型|URI|只读|示例|说明|
|:-|:-|:-|:-|:-|
|应用资源|/path|是|/Common/header.png|-|
|Cache|internal://cache/path|否|internal://cache/fetch-123456.png|-|
|Files|internal://files/path|否|internal://files/image/demo.png|-|
|Mass|internal://mass/path|否|internal://mass/video/demo.mp4|-|
|Temp|internal://tmp/path|是|internal://tmp/xxxxx|由系统动态生成|

URI 允许的字符是 ``0-9a-zA-Z_-./%`` （不包含引号），URI 中不能出现..，URI 支持目录结构，目录由斜线'/'分隔。

- internal URI 表示的是应用私有文件，即在指定 internal URI 时，无需指定应用标识，同一个 internal。
- URI 对于不同的应用会指向不同的文件。

### 资源和文件访问规则

应用资源路径分为绝对路径和相对路径，以"/"开头的路径表示绝对路径，比如 ``/Common/a.png``，不以"/"开头的路径是相对路径，比如 ``a.png`` 和 ``../Common/a.png`` 等。

应用资源文件分为代码文件和资源文件，代码文件是指 .njs/.nss/.nml
等包含代码的文件，其他文件则是资源文件，这类文件一般只当作数据来使用，比如图片、视频等。

1.  在代码文件中，导入其他代码文件时，使用相对路径，比如：../Common/component.nml。
2.  在代码文件中，引用资源文件（如：图片、视频等）时，一般情况下使用相对路径，比如：./abc.png。
3.  当代码文件需要被导入时，如果导入文件与被导入文件在同一个目录，被导入文件引用资源文件时可以使用相对路径，但如果不在同一目录，必须使用绝对路径，因为被导入文件编译时会被复制到导入文件中，编译后目录会发生变化。

比如 a.nss 文件被 b.nml 导入，如果 a.nss 与 b.nml 在同一个目录，a.nss 引用资源文件时可以写相对路径：abc.png；如果不在同一个目录，必须写绝对路径：/Common/abc.png。

再比如当 a.nml 文件被 b.nml 文件导入时，如果 a.nml 与 b.nml 在同一个目录，a.nml 引用资源文件时可以写相对路径：a.png；如果不在同一个目录，a.nml 引用资源必须写绝对路径：/Common/abc.png。

在 NSS 中，与前端开发一致，使用 url(PATH) 的方式访问资源文件，如：url(/Common/abc.png)
