# jinyue.chen

> test multipageStart

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

# vue多页面配置指南
1. 指定入口的js文件 （entry）
```js
entry: {
  entry1: '/url1/filename1.js',
  entry2: '/url2/filename2.js'
}
```

2. 指定编译所用的html模板 （plugin中的HtmlWebpackPlugin）
```js
// pulgin数组中可以接受多个new HtmlWebpackPlugin对象， 以下为dev模式下
new HtmlWebpackPlugin({
  filename: 'index.html',// 对应html模板的文件名
  template: 'index.html',// 对应html模板的路径
  inject: true
})

// 改为
let arr = []
arr.push(new HtmlWebpackPlugin({
  template: filePath,
  filename: filename + '.html',
  // 页面模板需要加对应的js脚本，如果不加这行则每个页面都会引入所有的js脚本
  chunks: ['manifest', 'vendor', filename],// you need attention
  inject: true
}))

plugin.concat(arr)// 可以用结构赋值 puglin: [xxxx, ...arr]
```

3. 修改 historyApiFallback（应该是对h5路由无法匹配的补充）
仅限于dev模式的路由无法匹配，build以后应该有后端controller控制
```js
historyApiFallback: {
  rewrites: [
    // { from: /.*/, to: path.posix.join(config.dev.assetsPublicPath, 'index.html') },
    { from: /\/$/, to: path.posix.join(config.dev.assetsPublicPath, 'index.html')},
    { from: /^\/login/, to: path.posix.join(config.dev.assetsPublicPath, 'login.html')},
    { from: /./, to: path.posix.join(config.dev.assetsPublicPath, '404.html')},
  ],
}
```

4. 页面代码
> 需要保证App.vue中的id和对应入口js和html模板的id保持一致
```js
// .html
<div id="xx"></div> 
// .js
new Vue({
  el: '#xx',
  components: { App },
  template: '<App/>'
})
// App.vue
<div id="xx"></div> 
```

#### 参考文章
- [使用Vue-cli搭建多页面时对项目结构和配置的调整](https://www.jianshu.com/p/0a30aca71b16)
- [简单构建ThinkJS+Vue2.0前后端分离的多页应用](https://www.h5jun.com/post/thinkjs-vue2.0-build.html)















