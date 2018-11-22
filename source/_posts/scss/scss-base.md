---
  title: SCSS基础
---

### 变量
  1. 揭秘: https://www.w3cplus.com/preprocessor/sass-basic-variable.html
  ```scss
    $width: 200px !default;
  ```
  2. 包含以下三部分:
    - 变量声名符：$
    - 变量名称
    - 变量的值
    - !default表示默认值
  3. 普通变量与默认变量
    - 普通变量：定义之后可以在全局范围内使用
    - 默认变量：用来设置默认值，在组件化开发中非常有用。在后面添加!default,然后根据需求来覆盖，覆盖方式是在默认变量之前重新声名下变量即可
  4. 变量的调用：声名变量后，在需要的地方直接调用即可
  5. 局部变量和全局变量
    - 局部变量：定义在元素内部，会覆盖全局变量
    - 全局变量：定义在元素外面
  6. 什么时候声明变量？
    - 该值至少重复出现了两次
    - 该值至少可能会被更新一次
    - 该值所有的表现都与变量有关
    - 不要声名一个永远不需要更新或者只在单一地方使用的变量

### 嵌套
  1. 选择器嵌套
    ```css
      nav a {
        color:red;
      }

      header nav a {
        color:green;
      }
    ```
    scss嵌套写法
    ```scss
      // & 置后表示父选择器为子集
      nav {
        a {
          color: red;
        }
        header & {
          color: green;
        }
      }
    ```

  2. 属性嵌套
    ```css
      .box {
        border-top: 1px solid red;
        border-bottom: 1px solid green;
      }
    ```
    scss嵌套写法
    ```scss
      .box {
        border: {
          top: 1px solid red;
          bottom: 1px solid green;
        }
      }
    ```
  3. 伪类嵌套
    ```css
      clearfix:before, .clearfix:after {
        content: "";
        display: table;
      }
      .clearfix:after {
        clear: both;
        overflow: hidden;
      }
    ```
    scss嵌套
    ```scss
      .clearfix{
        &:before,
        &:after {
          content:"";
          display: table;
        }
        &:after {
          clear:both;
          overflow: hidden;
        }
      }
    ```
    注意：避免选择器嵌套
    - 选择器嵌套最大的问题是将使最终的代码难以阅读。开发者需要花费巨大精力计算不同缩进级别下的选择器具体的表现效果
    - 选择器越具体则声明语句越冗长，而且对最近选择器的引用(&)也越频繁

### 混合宏（需要重复使用大段的样式时）
  1. 声明混合宏：@mixin
    - 不带参数混合宏
      ```scss
        @mixin border-radius {
          -webkit-border-radius: 5px;
          border-radius: 5px;
        }
      ```
    - 带参数的混合宏
      ```scss
        @mixin border-radius($radius: 5px) {
          -webkit-border-radius: $radius;
          border-radius: $radius;
        }
      ```
    - 复杂的混合宏（大括号里可以带逻辑关系）
      ```scss
        @mixin box-shadow($shadow...) {
          @if length($shadow) >= 1 {
            @include prefixer(box-shadow, $shadow);
          } @else{
            $shadow:0 0 4px rgba(0,0,0,.3);
            @include prefixer(box-shadow, $shadow);
          }
        }
      ```
  2. 混合宏的参数
    - 传一个不带值得参数, 调用时可以给混合宏传一个参数值
      ```scss
        @mixin border-radius($radius) {
          -webkit-border-radius: $radius;
          border-radius: $radius;
        }
      ```
    - 传一个带值得参数, 相当于有一个默认值，调用时不传参会直接取默认值
      ```scss
        @mixin border-radius($radius: 5px) {
          -webkit-border-radius: $radius;
          border-radius: $radius;
        }
      ```
    - 传多个参数, 当混合宏参数过多时，可以使用'...'代替
      ```scss
        @mixin size($width, $height) {
          width: $width;
          height: $height;
        }

        .box {
          @include size(500px, 300px)
        }
      ```
  3. 调用混合宏； @include
    ```scss
      button {
        @include border-radius;
      }
    ```
    编译结果
    ```css
      button {
        -webkit-border-radius: 5px;
        border-radius: 5px;
      }
    ```
    4. 不足之处
      - 生成冗余代码，不同的地方调用多次，不能智能的将相同的样式代码合在一起

### 继承 @extend
  ```scss
    btn {
      border: 1px solid #ccc;
      padding: 6px 10px;
      font-size: 14px;
    }

    .btn-primary {
      background-color: #f36;
      color: #fff;
      @extend .btn;
    }

    .btn-second {
      background-color: orange;
      color: #fff;
      @extend .btn;
    }
  ```
  scss编译后的结果
  ```css
    .btn, .btn-primary, .btn-second {
      border: 1px solid #ccc;
      padding: 6px 10px;
      font-size: 14px;
    }

    .btn-primary {
      background-color: #f36;
      color: #fff;
    }

    .btn-second {
      background-clor: orange;
      color: #fff;
    }
  ```

### 占位符 %placeholder
  1. 如果不被@extend调用，就不会产生任何代码。
  2. 通过@extend调用的占位符，会将相同的代码合并到一起。

### 混合宏 VS 继承 VS 占位符
  1. 混合宏：如果代码中涉及到变量，使用混合宏来创建相同的代码块
  2. 继承：如果代码块中不需要传任何变量参数，而且有一个基类已经在文件中存在了，使用继承
  3. 占位符：类似继承，不过不调用就不会在css中产生任何代码

### 插值
  1. 不能在@mixin语法中调用
  2. @extend中可以调用
  ```scss
    $properties: (margin, padding);
    @mixin set-value($side, $value) {
        @each $prop in $properties {
            #{$prop}-#{$side}: $value;
        }
    }
    .login-box {
        @include set-value(top, 14px);
    }
  ```
  编译出的css
  ```css
    .login-box {
      margin-top: 14px;
      padding-top: 14px;
    }
  ```

### 注释
  1. /* */
  2. //

### 数据类型
  1. 数字：1，2，3，10px
  2. 字符串：引号字符串和无引号字符串
    - 使用 #{ }插值语句 (interpolation) 时，有引号字符串将被编译为无引号字符串，这样方便了在混合指令 (mixin) 中引用选择器名
  3. 颜色：如：blue, #fff
  4. 布尔型：true, false
  5. 空置：null
  6. 值列表：用空格或者逗号分开

### 运算
  1. 加法/减法：在变量或属性中都可以做加法/减法运算
  2. 乘法：只能有一个单位，两个会报错
  3. 除法：/ 被当做除号的情况
    - 数值被圆括号包围
    - 数值或它的任意部分是存储在一个变量中或是函数的返回值
    - 数值是另一个数学表达式的一部分
  4. 变量运算
  5. 数字运算
  6. 字符运算





    

  

