---
title: SCSS进阶
---

### SCSS控制命令

1. @if: 根据条件来处理样式块，如果条件为true返回一个样式块，反之false返回另一个样式块。配合@else if 和 @else 一起使用

  ```Scss
    // 假设要控制一个元素的隐藏和显示，就可以定义一个混合宏，通过@if...@else...来判断传进参数的值来控制。
    @mixin blockOrHidden($boolean:true) {
      @if $boolean {
        display: block;
      }
      @else {
        display: none;
      }
    }

    .block {
      @include blockOrHidden;
    }
    .hidden {
      @include blockOrHidden(false);
    }
  ```
  编译出来的css
  ```css
    .block {
      display: block;
    }
    .hidden {
      display: none;
    }
  ```

2. @for: 循环
  - @for $i from <start> through <end> （包括end这个数）
  - @for $i from <start> to <end>（不包括end这个数）
    - $i 表示变量
    - start 表示起始值
    - end 表示结束值
  ```scss
    @for $i from 1 through 3 {
      .item-#{$i} {
        width: 2em * $i;
      }
    }
  ```
  @for 应用在网络系统生成各个格子class的代码

  ```scss
    $grid-prefix: span !default;
    $grid-width: 60px !default;
    $grid-gutter: 20px !default;

    %grid: {
      float: left;
      margin-left: $grid-gutter / 2;
      margin-right: $grid-gutter / 2;
    }

    @for $i from 1 through 12 {
      .#{$grid-prefix}#{$i} {
        width: $grid-width * $i + $grid-gutter * ($i - 1);
        @extend %grid;
      }
    }
  ```

3. @while循环：生成不同的样式块，直到表达式值为false时停止循环。

```scss
  $types: 4;
  $type-width: 20px;

  @width $types > 0 {
    .while-#{$types} {
      width: $type-width + $types;
    }
    $types: $types - 1;
  }
```

4. @each循环：遍历一个列表，然后从列表中取出对应的值
@each $var in <list>

```scss
  $list: adam john wynn mason kuror;

  @mixin author-images {
    @each $authorn in $list {
      .photo-#{$author} {
        background: url("/images/avatars/#{author}.png") no-repeat;
      }
    }
  }

  @author-bio {
    @include author-images;
  }
```

### SCSS函数
  - 字符串函数
  - 数字函数
  - 列表函数
  - 颜色函数
  - introspection函数
  - 三元函数

  #### 字符串函数
    1. unquote($string): 删除字符串中的引号
      - 只能删除最外层的
      - 如果没有带引号，则返回原始的字符串

    2. quote($string): 给字符串添加引号
      - 如果字符串带有引号会统一换成双引号
      - 字符串中间有单引号或者空格时，需要用单引号或双引号括起，否则编译的时候将会报错

    3. to-upper-case(): 将字符串小写字母转换成大写字母
      ```scss
        .test {
          text: to-upper-case(aaaa);
          text: to-upper-case(aA-aAAA-aaa)
        }
      ```
      ```css
        .test {
          text: AAAA;
          text: AA-AA-AAA;
        }
      ```
    3. to-lower-case(): 将字符串转换为小写字母
      ```scss
        .test {
          text: to-lower-case(AAAAA);
          text: to-lower-case(aA-aAAA-aaa);
        }
      ```
      ```css
        .test {
          text: aaaaa;
          text: aa-aaaa-aaa;
        }
      ```
  #### 数字函数
    1. percentage($value): 将一个不带单位的数转换成百分比，如果带有单位会报错
    2. round($value): 将一个数值四舍五入转为整数，可以携带单位的任何值
    3. ceil($value): 将大于自己的小数转换成下一位整数，只入不舍
    4. floor($value): 将一个数去除他的小数部分，只舍不入
    5. abs($value): 返回一个数的绝对值
    6. min($numbers...): 找出几个数之间的最小值，不能存在不同的单位
    7. max($numbers...): 找出几个数之间的最大值，不能存在不同的单位
    8. random(): 获取随机数
  
  #### 列表函数：主要包括一些对列表参数的使用
    1. length($list): 返回列表的长度
    2. nth($list, $n): 返回一个列表中指定的某个标签
    3. join($list1, $list2, [$separator]): 将两个列给连接在一起，变成一个列表
    4. append($list1, $val, [$separator]): 将某个值放在列表后面
      - separator的值：comma、space
    5. zip($lists...): 将几个列表结合成一个多维的列表，每一个单一的列表数值必须是相同的
    6. index($list, $value): 返回一个值在列表中的位置值

  #### introspection: 检测函数
    1. type-of($value): 返回一个值得类型
      - number: 数值型
      - string: 字符串型
      - bool: 布尔型
      - color: 颜色型
    2. unit($number): 返回一个值的单位
    3. unitless($number): 返回一个值是否带单位
    4. comparable($number-1, $number-2): 判断两个值是否可以做加减和合并

  #### Miscellnaeous: 三元条件函数
    if($condition, $if-true, $if-false)

  #### Map
    - 格式
      ```scss
        $map: (
          $key1: value1,
          $key2: value2,
          $key3: value3
        )
      ```
    - 实例: 可应用于换肤管理颜色变量
      ```scss
        $default-color: #fff !default;
        $primary-color: #22ae39 !default;

        // map等同上面的
        $color{
          default: #fff !default;
          pramary: #22ae39 !default;;
        }


      ```
    - 方法
      - map-get($map, $key): 根据指定的key值，返回map中相关的值.map中没有会返回null
        ```scss
          $social-colors: {
            driabble: #fff;
            facebook: #ccc;
            github: #171515;
            google: #db3377;
            twitter: #55acee;
          }
          .btn-driabble {
            color: map-get($social-colors, driabble)
          }
        ```
      - map-merge($map1,$map2): 将两个map合成一个map
        ```scss
          $color: (
            text: #f36;
            link: #f63;
            border: #ddd;
            background: #fff;
          )
          $type: (
            font-size: 12px;
            line-height: 1.6;
            border: #ccc;
            background: #000;
          )
          $newmap: map-merge($color,$type)
        ```
        scss编译
        ```css
          $newmap: (
            text: #f36;
            link: #f63;
            font-size: 12px;
            line-height: 1.6;
            border: #ccc;
            background: #000;
          )
        ```
      - map-remove($map,$key): 从map中删除一个key, 返回一个新的map
      - map-keys($map): 返回map中所有的key
      - map-values($map): 返回map中所有的value
      - map-has-key($map,$key): 根据给定的key值判断map是否有对应的value值，如果有返回true,否则返回false
      ```scss
          // 改进上面的代码
          $social-colors: {
            driabble: #fff;
            facebook: #ccc;
            github: #171515;
            google: #db3377;
            twitter: #55acee;
          }
          @if map-has-key($social-colors, driabble) {
            .btn-driabble {
              color: map-get($social-colors, driabble)
            }
          } @else {
            @warn "No color found for faceboo in $social-colors map.property ommitted."
          }
          
          // 改进
          @function colors($color) {
            @if not map-has-key($social-colors,$color) {
              @warn "No color found for '#{$color}' in $social-colors map.property ommitted."
            }
            @return map-get($social-colors, $color)
          }
          .btn-driabble {
            color: colors(driabble)
          }
        ```
      - keywords($args): 动态创建一个map, 返回一个函数的参考，这个参数可以动态的设置key和value
      
  #### 颜色函数
    1. RGB()
      - rgb($red,$green,$blue): 根据红绿蓝三个值创建颜色
      - rgba(): 将一个颜色根据透明度转换成 rgba 颜色
        - rgba($red,$green,$blue,$alpha)
        - rgba($color,$alpha)
      - red($color): 从一个颜色中获取其中红色值
      - green($color): 从一个颜色中获取其中绿色值
      - blue($color): 从一个颜色中获取其中蓝色值
      - mix($color-1,$color-2,[$weight]): 把两种颜色混合在一起
    2. HSL函数
      - hsl($hue,$saturation,$lightness)：通过色相（hue）、饱和度(saturation)和亮度（lightness）的值创建一个颜色
      - hsla($hue,$saturation,$lightness,$alpha)：通过色相（hue）、饱和度(saturation)、亮度（lightness）和透明（alpha）的值创建一个颜色
      - hue($color)：从一个颜色中获取色相（hue）值
      - saturation($color)：从一个颜色中获取饱和度（saturation）值
      - lightness($color)：从一个颜色中获取亮度（lightness）值
      - adjust-hue($color,$degrees)：通过改变一个颜色的色相值，创建一个新的颜色
      - lighten($color,$amount)：通过改变颜色的亮度值，让颜色变亮，创建一个新的颜色
      - darken($color,$amount)：通过改变颜色的亮度值，让颜色变暗，创建一个新的颜色
      - saturate($color,$amount)：通过改变颜色的饱和度值，让颜色更饱和，从而创建一个新的颜色
      - desaturate($color,$amount)：通过改变颜色的饱和度值，让颜色更少的饱和，从而创建出一个新的颜色
      - grayscale($color)：将一个颜色变成灰色，相当于desaturate($color,100%)
      - complement($color)：返回一个补充色，相当于adjust-hue($color,180deg)
      - invert($color)：反回一个反相色，红、绿、蓝色值倒过来，而透明度不变
    3. opacity函数
      - alpha($color)/opacity($color): 获取颜色透明度
      - rgba($color, rgba): 改变颜色透明度
      - opacify($color, $amount) / fade-in($color, $amount)：使颜色更不透明
      - transparentize($color, $amount) / fade-out($color, $amount)：使颜色更加透明
### @规则
  1. @import: 引入文件
    - 根据文件名引入。 默认情况下，它会寻找 Sass 文件并直接引入
    - 在少数几种情况下，它会被编译成 CSS 的 @import 规则： 
      - 如果文件的扩展名是 .css。
      - 如果文件名以 http:// 开头。
      - 如果文件名是 url()。
      - 如果 @import 包含了任何媒体查询（media queries）
  2. @media: 冒泡到外面
    ```scss
      .sidebar {
        width: 300px;
        @media screen and (orientation: landscape) {
          width: 500px;
        }
      }
    ```
    ```css
      .sidebar {
        width: 300px; 
      }
      @media screen and (orientation: landscape) {
        .sidebar {
          width: 500px; 
        } 
      }
    ```
  3. @extend: 扩展选择器或占位符
  4. @at-root: 当你选择器嵌套多层之后，想让某个选择器跳出
    ```scss
      .a {
        color: red;
        .b {
          color: orange;
          .c {
            color: yellow;
            @at-root .d {
              color: green;
            }
          }
        }  
      }
    ```
    ```css
      .a {
        color: red;
      }

      .a .b {
        color: orange;
      }

      .a .b .c {
        color: yellow;
      }

      .d {
        color: green;
      }
    ```
  5. @debug: 调试指令
    ```scss
      @debug 10em + 12em;
    ```
  6. @warn: 警告指令
    ```scss
      @warn "Assuming #{$x} to be in pixels";
    ```
  7. @error: 错误指令
    ```scss
      @error "你需要将#{$x}值设置在10以内的数";
    ```

### 实战--七色卡
  ```html
    <ul class="swatches red">
      <li></li>
        ...      
      <li></li>
    </ul>
    <ul class="swatches orange">
      <li></li>
        …
      <li></li>
    </ul>
    <ul class="swatches yellow">
      <li></li>
        …
      <li></li>
    </ul>
    <ul class="swatches green">
      <li></li>
            …
      <li></li>
    </ul>
    <ul class="swatches blue">
      <li></li>
          …
      <li></li>
    </ul>
    <ul class="swatches purple">
      <li></li>
          …
      <li></li>
    </ul>
  ```
  ```scss
    //定义变量
    $redBase: #DC143C;
    $orangeBase: saturate(lighten(adjust_hue($redBase, 39), 5), 7);//#f37a16
    $yellowBase: saturate(lighten(adjust_hue($redBase, 64), 6), 13);//#fbdc14
    $greenBase: desaturate(darken(adjust_hue($redBase, 102), 2), 11);//#73c620
    $blueBase: saturate(darken(adjust_hue($redBase, 201), 2), 1);//#12b7d4
    $purpleBase: saturate(darken(adjust_hue($redBase, 296), 2), 1);//#a012d4
    $blackBase: #777;
    $bgc: #fff;

    // 定义mixin
    //定义颜色变暗的 mixin
    @mixin swatchesDarken($color) {
        @for $i from 1 through 10 {
            $x:$i+11;
            li:nth-child(#{$x}) {
                $n:$i*5;
                $bgc:darken($color,$n); //颜色变暗
                background-color: $bgc;
    &:hover:before { //hover状态显示颜色编号
                    content: "#{$bgc}";
                    color: lighten($bgc,40);
                    font-family: verdana;
                    font-size: 8px;
                    padding: 2px;
                }
            }
        }
    }
    //定义颜色变亮的 mixin
    @mixin swatchesLighten($color) {
        @for $i from 1 through 10 {
            $x:11-$i;
            li:nth-child(#{$x}) {
                $n:$i*5;
                $bgc:lighten($color,$n);
                background-color: $bgc;
    &:hover:before {
                    content: "#{$bgc}";
                    color: darken($bgc,40);
                    font-family: verdana;
                    font-size: 8px;
                    padding: 2px;
                }
            }
        }
    }
    // 调用mixin
    .swatches li {    
      width: 4.7619047619%;
      float: left;
      height: 60px;
      list-style: none outside none;
    }

    ul.red {
      @include swatchesLighten($redBase);
      @include swatchesDarken($redBase);
      li:nth-child(11) {
          background-color: $redBase;
      }
    }

    ul.orange {
      @include swatchesLighten($orangeBase);
      @include swatchesDarken($orangeBase);
      li:nth-child(11) {
          background-color: $orangeBase;
      }
    }

    ul.yellow {
      @include swatchesLighten($yellowBase);
      @include swatchesDarken($yellowBase);
      li:nth-child(11) {
          background-color: $yellowBase;
      }
    }

    ul.green {
      @include swatchesLighten($greenBase);
      @include swatchesDarken($greenBase);
      li:nth-child(11) {
          background-color: $greenBase;
      }
    }

    ul.blue {
      @include swatchesLighten($blueBase);
      @include swatchesDarken($blueBase);
      li:nth-child(11) {
          background-color: $blueBase;
      }
    }

    ul.purple {
      @include swatchesLighten($purpleBase);
      @include swatchesDarken($purpleBase);
      li:nth-child(11) {
          background-color: $purpleBase;
      }
    }

    ul.black {
      @include swatchesLighten($blackBase);
      @include swatchesDarken($blackBase);
      li:nth-child(11) {
          background-color: $blackBase;
      }
    }
  ```

      



  
    
