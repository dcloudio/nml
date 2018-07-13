## HelloWorld
完成环境的搭建后，就可以愉快地开始 nml 的开发了。
### 创建项目
启动 HBuilderX，菜单->文件->新建->项目->uni-app->从模板选择->nml，输入项目名“HelloWorld”完成。 

这个项目已经包含了项目配置和简单页面的基本代码，项目根目录结构如下：
```
│  app.njs
│  manifest.json
│  
├─Common
│  │  logo.png
│  │  
│  ├─img
│  └─nss
│          common.nss
│          
├─Home
│      index.nml
│      
├─sign
│  │  README.md
│  │  
│  └─release
└─unpackage
```

- app.njs APP文件（用于包括公用资源）
- manifest.json 项目配置文件
- Common 公用的资源
- Home 首页
- sign 签名文件目录

### 真机运行
将手机设备连接到电脑，这时 HBuilderX 会自动检测连接到电脑上的设备。  
通过如下两种方式真机运行，可以在手机上查看效果。

- 菜单->运行->真机运行->设备
![](https://img-cdn-qiniu.dcloud.net.cn/uploads/images/1521636271142.png)
- 工具栏->真机运行按钮->设备
![](https://img-cdn-qiniu.dcloud.net.cn/uploads/images/1521636278086.png)


注意：

- 首次真机运行，会安装 HBuilderX 基座。
- 真机运行的状态下，每次修改项目文件，都会**自动同步**项目至手机设备。
