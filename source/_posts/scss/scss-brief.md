---
title: SCSS简介
---

### SCSS的安装方式

- gem: 安装所有的ruby应用；
- npm: 安装所有的node应用；
- brew: 安装所有的mac应用；

scss是ruby写的应用，所以安装方式要选择gem
1. gem install sass
2. sass -v检查是否安装
3. sass包下载到本地安装：gem install sass包路径xx

### 项目引入
scss只是一个预处理工具，最终项目里面要引用的实际上是：编译好的.css文件

### SCSS编译方法
1. 命令编译
  - 手动编译：sass 要编译的文件路径:要输出的文件路径
  - 自动编译(检测到修改之后会自动编译)：sass --watch 要编译的文件路径:要输出的文件路径

2. GUI工具编译
  - Koala: http://www.w3cplus.com/preprocessor/sass-gui-tool-koala.html
  - CodeKit: https://www.w3cplus.com/preprocessor/sass-gui-tool-codekit.html

3. 自动化编译
  - gulp
    ```js
      var gulp = require('gulp');
      var sass = require('gulp-sass');

      gulp.task('sass', function () {
          gulp.src('./scss/*.scss')
              .pipe(sass())
              .pipe(gulp.dest('./css'));
      });

      gulp.task('watch', function() {
          gulp.watch('scss/*.scss', ['sass']);
      });

      gulp.task('default', ['sass','watch']);
    ```
  - grunt

### SCSS编译输出不同样式风格
  ```scss
    nav {
      ul {
        margin: 0;
        padding: 0;
        list-style: none;
      }

      li { display: inline-block; }

      a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
      }
    }
  ```
  1. 嵌套输出 --style nested
    ```css
      nav ul {
        margin: 0;
        padding: 0;
        list-style: none; }
      nav li {
        display: inline-block; }
      nav a {
        display: block;
        padding: 6px 12px;
        text-decoration: none; }
    ```
  2. 展开输出 --style expanded (和 nested 类似，只是大括号在另起一行)
    ```css
      nav ul {
        margin: 0;
        padding: 0;
        list-style: none;
      }
      nav li {
        display: inline-block;
      }
      nav a {
        display: block;
        padding: 6px 12px;
        text-decoration: none;
      }
    ```
  3. 紧凑输出 --style compact
    ```css
      nav ul { margin: 0; padding: 0; list-style: none; }
      nav li { display: inline-block; }
      nav a { display: block; padding: 6px 12px; text-decoration: none; }
    ```
  4. 压缩输出 --style compressed (去掉标准的 Sass 和 CSS 注释及空格)
    ```css
      nav ul{margin:0;padding:0;list-style:none}nav li{display:inline-block}nav a{display:block;padding:6px 12px;text-decoration:none}
    ```

### SCSS常见编译错误
  1. 字符编译引起的，Scss编译过程不支持GBK编码，所以在创建文件的时候，需要将文件编码设置为"utf-8"
  2. 中文字符引起的，项目中文件命名以及目录命名都不要使用中文字符

### SCSS调试
  sourcemap
