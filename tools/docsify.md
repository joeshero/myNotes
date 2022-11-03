# Docsify

## docsify介绍

docsify是一款文档生成器，用来生成文档网站，所有的html文件都是运行时动态生成，所以只需要维护一些markdown文件就可以快速直接发布。</br>

docsify相比我之前用过的hexo来说比较轻量，参考官方文档[docsify](https://docsify.js.org/#/zh-cn/),它还支持多套主题，全文搜索等功能，
可以作为一个笔记工具，也可以发布到服务器做网站文档。

## 快速开始

以mac为例，操作步骤:

### 1、下载node.js

直接在官网下载node.js即可

### 2、npm替换为cnpm 

由于npm下载速度比较慢，替换为cnpm
```npm install -g cnpm --registry=https://registry.npm.taobao.org```

### 3、安装`docsify-cli`

```
npm i docsify-cli -g
```

### 4、初始化一个项目

```
docsify init ./docs
```

### 5. 本地启动

初始化项目后，目录结构为：

```
docs
 ├── index.html 入口文件
 ├── .nojekyll 用于阻止 GitHub Pages 忽略掉下划线开头的文件
 └── README.md 主页
```

运行`docsify serve docs`即可启动本地服务器，默认访问地址 http://localhost:3000。

## 定制化

docsify启动后，主页是一个空页面，一些侧边栏，全文搜索等功能需要自己手动加。

### 侧边栏

```
<script>
  window.$docsify = {
    // 加载侧边栏 _sidebar.md
    loadSidebar: true,
    subMaxLevel: 2,

    // 文档标题，会显示在侧边栏顶部
    name: '温锦瑜的笔记',
    // 点击文档标题后跳转的链接地址
    nameLink: '/',

    // 小屏设备下合并导航栏到侧边栏
    mergeNavbar: true,
  }
</script>
```

### 全文搜索

```
<script>
  window.$docsify = {
    // 搜索功能相关
    search: {
      maxAge: 86400000, // 过期时间，单位毫秒，默认一天
      paths: 'auto',
      placeholder: 'Type to search',
      noData: 'No Results!',
      depth: 6          // 搜索标题的最大程级, 1 - 6
    }
  }
</script>

<script src="//unpkg.com/docsify/lib/plugins/search.js"></script>
```

### 代码复制

```
<script>
  window.$docsify = {
    // 代码块点击复制
    copyCode: {
      buttonText : 'Copy',
      errorText  : 'Error',
      successText: 'Copied'
    }
  }
</script>
<script src="//unpkg.com/docsify-copy-code"></script>
```

### 字数统计

```
window.$docsify = {
  count:{
    countable:true,
    fontsize:'0.9em',
    color:'rgb(90,90,90)',
    language:'chinese'
  }
}

<script src="//unpkg.com/docsify-count/dist/countable.js"></script>
```