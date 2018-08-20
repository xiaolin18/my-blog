title: 图片适配PC、移动端
---
<img src="../../../../../images/img-responsive.jpeg" style="width: 720px;height: 400px" alt="响应式图片">

### 相关链接

[移动端适配方案](https://juejin.im/entry/56ce78eac24aa800545af276)

### 问题
1. 什么是自适应？

2. PC、移动端适配方案？

3. 图片放大、缩放会导致变形模糊

4. 图片加载缓慢，如何改善

### 百分比布局

* 因为pc端长>高，移动端长<高，所以可以有两张图片。根据视口的宽高比例来决定操作展示那一张图片
```JavaScript
  长/高 > 1 ==> 显示长 > 高的图片
  长/高 <= 1 ==> 显示长 < 高的图片
```
* img标签引入的图片，可以使用延迟加载的方式来加载，在实际加载图片之前先用js检查窗口宽度，然后加载不同分辨率的图片

```HTML
  <div id="box">
    <img id="img" class="box-content" alt="响应式图片">
  </div>
```

```CSS
  body {
    margin: 0 auto;
    padding: 0 auto;
  }
  #box {
    margin: 0 auto;
    width: 100%;
    height: 100%;
    overflow: hidden;
    cursor: pointer;
  }
  .box-content {
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  }
```

```JavaScript
  var img = document.getElementById('img')
  var offsetWid = document.documentElement.clientWidth;
  var offsetHei = document.documentElement.clientHeight;
  let rat = offsetWid/offsetHei
  if (rat < 1) {
    img.setAttribute('src', './bg1.png')
  } else {
    img.setAttribute('src', './bg.jpg')
  }

  var btn = document.getElementById("box");
  btn.onclick = function () {
    // 当前页面打开
    window.location.href="https://www.dajie.com/corp/6830864/"
  }
```
