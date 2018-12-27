---
title: 作用域
---

# 概念
  - JS是脚本语言，只有runtime, 没有buildTime。是由各个浏览器引擎编译运行起来的

# 分类

  - 静态作用域（词法作用域）: js中的变量都有静态作用域，在编译阶段确定作用域，不会被函数的位置所改变。变量的作用域是变量可以被访问的区域。
  
  ```js
    var value = 1;
    function foo() {
        console.log(value);
    }

    function bar() {
        var value = 2;
        foo();
    }

    bar();
  ```
  - 嵌套作用域：如果一个变量的直接作用域嵌套了多层作用域，则变量在所有的作用域都可以被访问到
  ```js
    function foo (arg) {
      function bar() {
        console.log( 'arg:' + arg );
      }
      bar();
    }

    console.log(foo('hello'));   // arg:hello
  ```
  - 覆盖作用域: 内部作用域的变量与外部作用域的变量名相同，内部作用域就无法访问到外部的变量
  ```js
    var x = "global"；
    function f() {
      var x = "local"；
      console.log(x);   // local
    }

    f();
    console.log(x);  // global
  ```