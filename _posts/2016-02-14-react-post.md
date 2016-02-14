---
layout: post
title: 结合gulp和browserify和node构建react项目
cover: cover.jpg
date:   2016-02-14 15:51:06 +0800
categories: posts
---

刚开始只是想简单用node和gulp搭建一个react项目，简单介绍下步骤和我遇到的问题

### 准备工作
这里我用的是express框架搭建node项目
安装 express应用生成器

`$ npm install express-generator@4 -g`

express创建项目

`express react-reflux-simple-todo`

`cd react-reflux-simple-todo`

安装依赖

`npm install`


### 添加view

1.添加静态文件访问的路径(不添加将无法访问public下的静态资源)

`app.use('/public', express.static(path.join(__dirname, 'public')));`

2.修改视图的模版引擎，默认是jade,改为html,并且添加依赖ejs

`app.engine('html', require('ejs').renderFile)`

3.在views目录下添加index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>react demo</title>
</head>
<body>
    <div id="app"></div>
    <!-- 视图渲染入口 client.js -->
    <script src="client/client.js"></script>
</body>
</html>
```

### 添加客户端js

添加react依赖

```javascript
{
  "react": "~0.14.7",
  "react-tools": "~0.13.3",
  "react-dom": "~0.14.7",
}
```

1.添加client文件夹，新建client.js文件

```javascript
var React = require('react');
var ReactDOM = require('react-dom');
var CommentBox = React.createClass({
  render: function() {
    return(
      <div>
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
ReactDOM.render(
  <CommentBox />,
  document.getElementById('app')
);
```
这个时候运行项目，还需要将jsx解析成javascript，官网推荐使用reactify,下面将重点介绍
gulp和browserfy的完美结合.


#### 添加项目依赖的npm包
在package.json里添加依赖，各个npm插件就不一一介绍，主要介绍下reactify和browserify

```json
 {
  "browserify": "~4.2.3",
  "reactify": "~1.1.1",
  "gulp":"~3.9.0 ",
  "vinyl-source-stream": "~1.1.0",
  "gulp-rename": "~1.2.2",
  "babelify": "~7.2.0",
  "gulp-uglify": "~1.5.1",
  "gulp-load-plugins": "~1.2.0",
  "vinyl-buffer": "~1.0.0"
}
```
新建gulpfile.js文件


### browserify
![](http://i13.tietuku.com/c0c33791eacc0504.png)

最早也是最有名的前端模块管理器，众所周知，非RequireJS莫属。Require.js的问题在于各种参数设置过于繁琐，不容易学习，很难完全掌握。而且，实际应用中，往往还需要在服务器端，将所有模块合并后，再统一加载，这多出了很多工作量。browserify的出现很好的弥补了requireJs这一缺陷。

Browserify本身不是模块管理器，它只是能让 Node 模块跑在浏览器里，通过如下的命令将CMD格式的js直接生成能在浏览器运行的js

`$ browserify test.js > bundle.js`

`<script src="bundle.js"></script>`


### 在 Gulp 中使用 Browserify
browserify有个强大的功能——transform，将各种语言进行预处理，最后打包成bundle.js,例如coffee script、jsx.

看代码说事 gulpfile.js

```javascript
var browserify = require('browserify');  
var gulp = require('gulp');  
var uglify = require('gulp-uglify');  
var source = require('vinyl-source-stream');  
var buffer = require('vinyl-buffer');
var reactify = require('reactify');

gulp.task('browserify', function() {  
  return browserify('./src/js/app.js') //源文件路径
    .bundle()
    .transform(reactify)
    .pipe(source('bundle.js')) // 将常规流转换为包含 Stream 的 vinyl 对象
    .pipe(buffer())  //将 vinyl 对象内容中的 Stream 转换为 Buffe
    .pipe(uglify()) //压缩处理
    .pipe(gulp.dest('./dist/js')); //目的文件路径
});
```

### reactify
reactify主要是用来转换jsx，通过browserify的transform方法，将jsx语言转换成js

结合gulp,最终gulpfile.js代码如下:

```javascript
var gulp = require('gulp');
var browserify = require('browserify');
var reactify = require('reactify');
var $ = require('gulp-load-plugins')();
var source = require('vinyl-source-stream');
var buffer = require('vinyl-buffer');
var uglify = require('gulp-uglify');

gulp.task('jsx', function(){
    var b = browserify({
      entries: './client/client.js',
      debug: true,
      external: [],
      standalone: 'POP',
      cache: {},
   });

    b.transform(reactify);
    return b.bundle()
      .pipe(source('bundle.js'))
      .pipe(buffer())
      .pipe($.uglify())
      .pipe(gulp.dest('./public/build/'));
});

```
修改下index.html文件,引入bundle.js

`<script src="../public/build/bundle.js"></script>`

package.json文件添加scripts

```
"scripts": {
  "prestart": "npm run-script build",
  "build": "gulp jsx",
  "start": "node ./bin/www"
}
```

### 运行项目
`npm install`
`npm start`
打开浏览器访问[http://localhost:3000/](http://localhost:3000/)
