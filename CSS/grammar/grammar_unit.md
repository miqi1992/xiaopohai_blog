#CSS单位
> 本文分为绝对长度单位和相对长度单位来介绍CSS中的长度单位的主要知识。

### 绝对长度单位

绝对长度单位单标一个屋里测量。

#### 像素px(pixels)

  在web上，像素px是典型的度量单位，很多长度单位直接映射成像素。最终，他们被按照像素处理。

#### 英寸in(inches)

1in = 2.54cm = 96px

#### 厘米cm(centimeters)

1cm = 10mm = 96px/2.54 = 37.8px

#### 1/4毫米q(quarter-millimeters)

1q = 1/4mm = 0.954px

#### 点pt(points)

1pt = 1/72in ~= 0.0139in = 1/722.54cm = 1/7296px = 1.33px

#### 派卡pc(picas)

1pc = 12pt = 1/6in = 1/6\*96px = 16px

### 字体相关相对长度单位

em、ex、ch、rem是字体相关的相对长度单位

#### em
em表示元素的font-size属性的计算值，如果用于font-size属性本身，相对于父元素的font-size;若用于其它属性，相对于本身元素的font-size

```html
<style type="text/css">
.box {
font-size: 20px;
}
.in {
/*相对于父元素，所以2*20px=40px*/
font-size: 2em;
/*相对于本身元素，所以5*40px=200px */
height: 5em;

/*10*40px = 400px*/
width: 10em;
background-color: lightblue;
}
</style>

<body>
<div class="box">
<div class="in">测试文字</div>
</div>
</body>
```

<iframe style="width: 100%; height: 220px;" src="https://demo.xiaohuochai.site/css/base/b1.html" frameborder="0" width="320" height="240"></iframe>