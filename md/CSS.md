> ### css 盒模型

- css 盒模型本质是一个盒子，封装周围的 HTML 元素，它包括：边距，边框，填充和实际内容。

  - 盒子宽高计算如下：

    - 标准盒模型

      盒子大小 = content + border + padding +margin

    - 怪异盒模型(IE 盒模型)

      盒子大小 = content(width+border+padding) + margin

  - 如何选择盒模型

    如果是定义了完整的 doctype 的标准文档类型，无论是哪种模型情况，最终都会触发标准模式， (<!DOCTYPE html>)

    如果 doctype 协议缺失，会由浏览器自己界定，在 IE 浏览器中 IE9 以下（IE6.IE7.IE8）的版本触发怪异模式，其他浏览器中会默认为 W3c 标准模式。

  - 使用 box-sizing

        content-box： 默认值，border和padding不算到width范围内，可以理解为是W3c的标准模型(default)

        border-box：border和padding划归到width范围内，可以理解为是IE的怪异盒模型

        padding-box：将padding算入width范围

  - 什么情况下使用怪异盒模型

    当我们想让盒子的 width 变为 content+padding+border 的时候。比如我定义了 3 个元素，希望他们并排显示，宽度是 33.35,我再加 20px 的 padding 值，标准盒模型此时计算宽度就会是 33.3%+20px 的 padding 值。这样肯定会被挤下去。所以我们使用怪异盒模型。33.3% = width + padding + border.

> ### BFC

BFC 全称是块级格式上下文 (Block Formatting Context) 。

BFC 是一个独立的布局环境，其中的子元素布局是不受外界的影响，同时子元素不会影响到外面元素，

- 实现 BFC

  - 根元素 HTML

  - float 属性不为 none (left,right)

  - position 属性为 absolute,fixed

  - display 值为 inline-block、table-cell、table-caption、table、inline-table、flex、inline-flex、grid、inline-grid

  - overflow 属性不为 visible,为 hidden，scroll，auto

- BFC 区域的约束规则

  - 属于同一 BFC 的子元素垂直排列

  - 属于同一 BFC 的子元素的 margin 会重叠

  - 生成 BFC 元素的子元素中，每一个子元素左外边距与包含块的左边界相接触（对于从右到左的格式化，右外边距接触右边界），即使浮动元素也是如此（尽管子元素的内容区域会由于浮动而压缩），除非这个子元素也创建了一个新的 BFC（如它自身也是一个浮动元素）。

  - BFC 的区域不会与 float 的元素区域重叠

  - 计算 BFC 的高度时，浮动子元素也参与计算

  - 文字层不会被浮动层覆盖，环绕于周围

- BFC 解决的问题

  - 清除浮动

  - 阻止 margin 重叠

  - 实现自适应两栏，三栏布局

  - 可以阻止元素被浮动元素覆盖

> ### CSS优先级确定

- !important > 行内样式 > #id > .class > tag(属性选择器) > 标签选择器 > 通配选择器 > 继承 > 默认
- 选择器 从右往左 解析

> ### 层叠上下文

![cengdie](https://user-gold-cdn.xitu.io/2019/2/14/168e9d9f3a1d368b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



> ### 居中


> ### 实现两栏布局

- 方法一 ： 利用 float 和 BFC

```
  //CSS
  body {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
  }
  .left {
    float: left;
    width: 200px;
    height: 200px;
    background: tomato;
  }

  .right {
    overflow: auto;
    height: 200px;
    background: #efefef;
  }
//HTML
  <body>
    <div class="left"></div>
    <div class="right"></div>
  </body>
```

- 方法二 使用 float+ margin-left

```
//CSS
.left{
  width: 200px;
  height: 100px;
  background: tomato;
  float: left;
}

.right{
  margin-left: 200px;
  height: 100px;
  background: #efefef;
}
//HTML
<body>
    <div class="left"></div>
    <div class="right"></div>
</body>
```

- 方法三 使用 absolute，margin-left

```
//CSS
body {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
    position： relative;
}
.left {
  width: 200px;
  height: 100px;
  background: tomato;
  position: absolute;
  top: 0;
  left: 0;
}

.right {
  margin-left: 200px;
  height: 100px;
  background: #efefef;
}
//HTML
<body>
    <div class="left"></div>
    <div class="right"></div>
</body>
```

- 方法四 弹性盒布局

```
  //css
    .father-box{
        display: flex;
    }

    .left {
        width: 200px;
        height: 100px;
        background: tomato;
    }

    .right {
        flex: 1;
        height: 100px;
        background: #efefef;
    }
  //HTML
  <body>
    <div class="father-box">
        <div class="left"></div>
        <div class="right"></div>
    </div>
</body>
```

> ### 三栏布局

- 利用 position 布局

```
//CSS
      * {
            margin: 0;
            padding: 0;
        }

        body {
            min-width: 550px;
        }

        .container {
            position: relative;
        }

        .column {
            position: absolute;
            top: 0;
            min-height: 100px;
        }

        .left {
            left: 0;
            width: 200px;
            background: #ffbbff;
        }

        .center {
            left: 200px;
            right: 150px;
            background: #bfefff;
        }

        .right {
            right: 0;
            width: 150px;
            background: #eeee00;
        }
//HTML
<body>
<!-- 缺点是三栏高度不统一。 -->
    <div class="container">
        <div class="column left">left</div>
        <div class="column center">center</div>
        <div class="column right">right</div>
    </div>
</body>
```

- 利用 float 布局

```
//CSS
      * {
            margin: 0;
            padding: 0;
        }

        body {
            width: 100%;
            min-width: 550px;
        }

        .container {
            position: relative;
        }

        .column {
            min-height: 100px;
        }

        .left {
            float: left;
            width: 200px;
            background: #ffbbff;
        }

        .center {
            margin: 0 150px 0 200px;
            background: #bfefff;
        }

        .right {
            float: right;
            width: 150px;
            background: #eeee00;
        }
//HTML
 <!-- 这种方法的缺点是三栏高度不统一，center区域的内容要在最后渲染。否则右边会在第二行显示 -->
    <div class="container">
        <div class="column left">left</div>
        <div class="column right">right</div>
    <div class="column center">center</div>
</div>
```

- 利用 flex 布局

```
<!-- CSS -->
    * {
      margin: 0;
      padding: 0;
    }
    body {
      min-width: 550px;
    }
    .container {
      display: flex;
      justify-content: space-between;
    }
    .column {
      min-height: 100px;
    }
    .left {
      order: 1;
      width: 200px;
      background: #ffbbff;
    }
    .center {
      order: 2;
      flex: 1;
      background: #bfefff;
    }
    .right {
      order: 3;
      width: 150px;
      background: #eeee00;
    }
<!-- HTML -->
<div class="container">
    <div class="column center">center</div>
    <div class="column left">left</div>
    <div class="column right">right</div>
</div>
```

- Grid 布局

```
<!-- CSS -->
    * {
      margin: 0;
      padding: 0;
    }
    .container {
      display: grid;
      grid-template-columns: 200px auto 150px;
      width: 100%;
      min-height: 100px;
    }
    .left {
      grid-row: 1/2;
      grid-column: 1/2;
      background: #ffbbff;
    }
    .center {
      grid-row: 1/2;
      grid-column: 2/3;
      background: #bfefff;
    }
    .right {
      grid-row: 1/2;
      grid-column: 3/4;
      background: #eeee00;
    }
<!-- HTML -->
 <div class="container">
    <div class="column left">left</div>
    <div class="column center">center</div>
    <div class="column right">right</div>
  </div>
```

- 利用 tabel-cell 布局

```
<!-- CSS -->
    * {
      margin: 0;
      padding: 0;
    }
    .column {
      display: table-cell;
      height: 100px;
      min-height: 100px;
    }
    .left {
      width: 200px;
      min-width: 200px;
      background: #ffbbff;
    }
    .center {
      width: 100%;
      background: #bfefff;
    }
    .right {
      width: 150px;
      min-width: 150px;
      background: #eeee00;
    }
<!-- HTML -->
  <div class="container">
    <div class="column left">left</div>
    <div class="column center">center</div>
    <div class="column right">right</div>
  </div>
```

> ###  圣杯布局和双飞翼布局