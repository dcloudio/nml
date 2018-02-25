## nml 文件
APP，页面和自定义组件均通过 nml 文件编写，nml 文件由 template 模板、Style 样式和 script 脚本3个部分组成，一个典型的页面 nml 文件示例如下：
```html
<template>
  <div class="container" onclick="press">
    <text class="{{title}}" style="font-size:14px;">Hello {{title}}</text>
  </div>
</template>

<style>
  .container {
    flex-direction: column;
    justify-items: center;
  }
  
  .title {
    color: red;
  }
</style>

<script>
  module.exports = {
    props: ['title'],
    data: function() {
      return {
        title: 'World'
      }
    },
    press: function(e) {
      this.title = 'XXX'
    },
    onInit: function() {
      this.title = this.$app.$data.appName;
    }
  }
</script>
```

### 1.3.1 app.njs

当前 app.njs 编译后会包含 manifest 配置信息（可以在npm run build之后查看文件内容），所以请不要删除``/**manifest**/``的注释内容标识。

您可以在 ``<script>`` 中引入一些公共的脚本，并暴露在当前 app 的对象上，如下所示，然后就可以在页面 nml 文件的 ViewModel 中，通过 ``this.$app.util`` 访问。
```html
<script>
  import util from './util.js'

  module.exports = {
    /**manifest**/
    util: util
  }
</script>
```