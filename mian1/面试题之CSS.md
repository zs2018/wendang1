### 1、用三种方式实现块框的垂直剧中效果，假设框的高度根据内容自适应

注意兼容性，autoprefixer可以处理前缀的兼容而已,对于不支持transform和flex的浏览器，如果要兼容该部分浏览器，请选择其他方式（笔者做的大多是移动端，以下三种，除安卓4.3以下原生内核不支持外，并外见兼容性的bug,flex在X5内核，ios低版本内核中有些兼容性(如flex-wrap),需注意）。

第一种：flex

```
父元素设置以下属性
display:flex;
flex-direction:row //web 默认的值，rn默认column
align-items:center
```

第二种：absolute

```
父元素设置
position:relative
本元素
position:absolute;
top:50%;
transform:translateY(-50%);
```

第三种：利用display:table-cell属性

```
display:table-cell;
vertical-alian:middle;
```



### 2、offsetWidth、clientWidth、screenWidth区别



### 3、CSS3动画



### 4、选择器权重及优先级



### 5、CSS3新属性



### 6、盒模型



### 7、常用居中方法

居中在布局中很常见，我们假设DOM文档结构如下，子元素要在父元素中居中：

```
<div class="parent">
    <div class="child"></div>
</div>
```

#### 水平居中

子元素为行内元素还是块状元素，宽度一定还是宽度未定，采取的布局方案不同。下面进行分析：

**行内元素**：对父元素设置`text-align:center;`
**定宽块状元素**: 设置左右`margin`值为`auto`;
**不定宽块状元素**: 设置子元素为`display:inline`,然后在父元素上设置`text-align:center`; 
**通用方案**: flex布局，对父元素设置`display:flex;justify-content:center;`

#### 垂直居中

垂直居中对于子元素是单行内联文本、多行内联文本以及块状元素采用的方案是不同的。

**父元素一定，子元素为单行内联文本**：设置父元素的`height`等于行高`line-height`
**父元素一定，子元素为多行内联文本**：设置父元素的`display:table-cell`或`inline-block`，再设置`vertical-align:middle`;
**块状元素**:设置子元素`position:fixed（absolute）`，然后设置`margin:auto`;
**通用方案**: flex布局，给父元素设置`{display:flex; align-items:center;}`。

### 8 单列布局

![](https://segmentfault.com/img/remote/1460000008789042?w=948&h=488/view)

特征：定宽、水平居中

常见的单列布局有两种：

- 一种是`header`、`content`、`footer`宽度都相同，其一般不会占满浏览器的最宽宽度，但当浏览器宽度缩小低于其最大宽度时，宽度会自适应。
- 一种是`header`、`footer`宽度为浏览器宽度，但`content`以及`header`和`footer`里的内容却不会占满浏览器宽度。

对于第一种，对`header`、`content`、`footer`统一设置`width`或`max-width`，并通过`margin:auto`实现居中。

**DOM文档**:

```
<div class="layout">
  <div id="header">头部</div>
  <div id="content">内容</div>
  <div id="footer">尾部</div>
</div>
```

**CSS清单**:

```
  .layout{
  /*   width: 960px; *//*设置width当浏览器窗口宽度小于960px时，单列布局不会自适应。*/
    max-width: 960px;
    margin: 0 auto;
  }
```

对于第二种，`header`、`footer`的内容宽度为100%，但`header`、`footer`的内容区以及`content`统一设置`max-width`，并通过`margin:auto`实现居中。

**DOM文档**:

```
<div id="header">
  <div class="layout">头部</div>
</div>
<div id="content" class="layout">内容</div>
<div id="footer">
  <div class="layout">尾部</div>
</div>
```

**CSS清单**:

```
  .layout{
  /*   width: 960px; *//*设置width当浏览器窗口宽度小于960px时，单列布局不会自适应。*/
    max-width: 960px;
    margin: 0 auto;
  }
```

### 9 二列&三列布局

![](https://segmentfault.com/img/remote/1460000008789043?w=1388&h=753)

二列布局的特征是侧栏固定宽度，主栏自适应宽度。
三列布局的特征是两侧两列固定宽度，中间列自适应宽度。

之所以将二列布局和三列布局写在一起，是因为二列布局可以看做去掉一个侧栏的三列布局，其布局的思想有异曲同工之妙。对于传统的实现方法，主要讨论上图中前三种布局，经典的带有侧栏的二栏布局以及带有左右侧栏的三栏布局，对于flex布局，实现了上图的五种布局。

#### a. float+margin

**原理说明**：设置两个侧栏分别向左向右浮动，中间列通过外边距给两个侧栏腾出空间，中间列的宽度根据浏览器窗口自适应。

**DOM文档**:

```
<div id="content">
    <div class="sub">sub</div>
    <div class="extra">extra</div>
    <div class="main">main</div>
</div>
```

**布局步骤**:

1. 对两边侧栏分别设置宽度，并对左侧栏添加左浮动，对右侧栏添加有浮动。
2. 对主面板设置左右外边距，margin-left的值为左侧栏的宽度，margin-right的值为右侧栏的宽度。

**CSS清单**:

```
.sub{
    width: 100px;
    float: left;
}
.extra{
    width: 200px;
    float: right;
}
.main{
    margin-left: 100px; 
    margin-right: 200px;
}
```

**一些说明**:

*　注意DOM文档的书写顺序，先写两侧栏，再写主面板，更换后则侧栏会被挤到下一列（圣杯布局和双飞翼布局都会用到）。
*　这种布局方式比较简单明了，但缺点是渲染时先渲染了侧边栏，而不是比较重要的主面板。

**二列的实现方法**

如果是左边带有侧栏的二栏布局，则去掉右侧栏，不要设置主面板的`margin-right`值，其他操作相同。反之亦然。

#### b. position+margin

**原理说明**：通过绝对定位将两个侧栏固定，同样通过外边距给两个侧栏腾出空间，中间列自适应。

**DOM文档**:

```
<div class="sub">left</div>
<div class="main">main</div>
<div class="extra">right</div>
```

**布局步骤**:

1. 对两边侧栏分别设置宽度，设置定位方式为绝对定位。
2. 设置两侧栏的top值都为0，设置左侧栏的left值为0， 右侧栏的right值为0。
3. 对主面板设置左右外边距，margin-left的值为左侧栏的宽度，margin-right的值为右侧栏的宽度。

**CSS清单**:

```
.sub, .extra {
    position: absolute;
    top: 0; 
    width: 200px;
}
.sub { 
    left: 0;
}
.extra { 
    right: 0; 
}
.main { 
    margin: 0 200px;
}
```

**一些说明**:

- 与上一种方法相比，本种方法是通过定位来实现侧栏的位置固定。
- 如果中间栏含有最小宽度限制，或是含有宽度的内部元素，则浏览器窗口小到一定程度，主面板与侧栏会发生重叠。

**二列的实现方法**

如果是左边带有侧栏的二栏布局，则去掉右侧栏，不要设置主面板的`margin-right`值，其他操作相同。反之亦然。

#### c. 圣杯布局(float + 负margin + padding + position)

**原理说明**：

主面板设置宽度为100%，主面板与两个侧栏都设置浮动，常见为左浮动，这时两个侧栏会被主面板挤下去。通过负边距将浮动的侧栏拉上来，左侧栏的负边距为100%，刚好是窗口的宽度，因此会从主面板下面的左边跑到与主面板对齐的左边，右侧栏此时浮动在主面板下面的左边，设置负边距为负的自身宽度刚好浮动到主面板对齐的右边。为了避免侧栏遮挡主面板内容，在外层设置左右padding值为左右侧栏的宽度，给侧栏腾出空间，此时主面板的宽度减小。由于侧栏的负margin都是相对主面板的，两个侧栏并不会像我们理想中的停靠在左右两边，而是跟着缩小的主面板一起向中间靠拢。此时使用相对布局，调整两个侧栏到相应的位置。

**DOM文档**:

```
 <div id="bd">         
    <div class="main"></div>        
    <div class="sub"></div>        
    <div class="extra"></div>  
</div> 
```

**布局步骤**:

1. 三者都设置向左浮动。
2. 设置main宽度为100%，设置两侧栏的宽度。
3. 设置 负边距，sub设置负左边距为100%，extra设置负左边距为负的自身宽度。
4. 设置main的padding值给左右两个子面板留出空间。
5. 设置两个子面板为相对定位，sub的left值为负的sub宽度，extra的right值为负的extra宽度。

**CSS清单**:

```
.main {        
    float: left;       
    width: 100%;   
 }  
 .sub {       
    float: left;        
    width: 190px;        
    margin-left: -100%;               
    position: relative;  
    left: -190px;  
}   
.extra {        
    float: left;        
    width: 230px;        
    margin-left: -230px; 
    position: relative; 
    right: -230px;  
 }
#bd {        
    padding: 0 230px 0 190px;   
 }
```

**一些说明**

- DOM元素的书写顺序不得更改。
- 当面板的`main`内容部分比两边的子面板宽度小的时候，布局就会乱掉。可以通过设置`main`的`min-width`属性或使用双飞翼布局避免问题。

**二列的实现方法**

如果是左边带有侧栏的二栏布局，则去掉右侧栏，不要设置主面板的`padding-right`值，其他操作相同。反之亦然。

#### d. 双飞翼布局(float + 负margin + margin)

**原理说明**：

双飞翼布局和圣杯布局的思想有些相似，都利用了浮动和负边距，但双飞翼布局在圣杯布局上做了改进，在`main`元素上加了一层div, 并设置`margin`,由于两侧栏的负边距都是相对于main-wrap而言，main的margin值变化便不会影响两个侧栏，因此省掉了对两侧栏设置相对布局的步骤。

**DOM文档**:

```
<div id="main-wrap" class="column">
      <div id="main">#main</div>
</div>
<div class="sub"></div>        
<div class="extra"></div>
```

**布局步骤**:

1. 三者都设置向左浮动。
2. 设置main-wrap宽度为100%，设置两个侧栏的宽度。
3. 设置 负边距，sub设置负左边距为100%，extra设置负左边距为负的自身宽度。
4. 设置main的margin值给左右两个子面板留出空间。

**CSS清单**:

```
.main-wrap {        
    float: left;       
    width: 100%;   
 }  
 .sub {       
    float: left;        
    width: 190px;        
    margin-left: -100%;   
}   
.extra {        
    float: left;        
    width: 230px;        
    margin-left: -230px; 
 }
.main {    
    margin: 0 230px 0 190px;
}
```

**一些说明**

- **圣杯采用的是padding，而双飞翼采用的margin**，解决了圣杯布局main的最小宽度不能小于左侧栏的缺点。
- 双飞翼布局不用设置相对布局，以及对应的left和right值。
- 通过引入相对布局，可以实现三栏布局的各种组合，例如对右侧栏设置`position: relative; left: 190px; `,可以实现sub+extra+main的布局。

**二列的实现方法**

如果是左边带有侧栏的二栏布局，则去掉右侧栏，不要设置`main-wrap`的`margin-right`值，其他操作相同。反之亦然。

#### e. flex布局

如果你还没有学习flex布局，阮一峰老师的两篇博文将会很适合你



### 10清除浮动

1，父级div定义 height 

2，结尾处加空div标签 clear:both 

3，父级div定义 伪类:after 和 zoom 

4, 父级div定义 伪类:after 和 zoom 

```
.clearfloat:after{display:block;clear:both;content:"";visibility:hidden;height:0} 
.clearfloat{zoom:1} 
```

5, 父级div定义 overflow:hidden .必须定义width或zoom:1，同时不能定义height，使用overflow:hidden

6, 父级div定义 overflow:auto 必须定义width或zoom:1，同时不能定义height

7, 父级div 也一起浮动 

8, 父级div定义 display:table 

9, ，结尾处加 br标签 clear:both 

### 11  css 动画

```
animation: name duration timing-function delay iteration-count direction;
```

| 值                          | 描述                                     |
| --------------------------- | ---------------------------------------- |
| *animation-name*            | 规定需要绑定到选择器的 keyframe 名称。。 |
| *animation-duration*        | 规定完成动画所花费的时间，以秒或毫秒计。 |
| *animation-timing-function* | 规定动画的速度曲线。                     |
| *animation-delay*           | 规定在动画开始之前的延迟。               |
| *animation-iteration-count* | 规定动画应该播放的次数。                 |
| *animation-direction*       | 规定是否应该轮流反向播放动画。           |

```
div
{
width:100px;
height:100px;
background:red;
animation:myfirst 5s;
-moz-animation:myfirst 5s; /* Firefox */
-webkit-animation:myfirst 5s; /* Safari and Chrome */
-o-animation:myfirst 5s; /* Opera */
}

@keyframes myfirst
{
from {background:red;}
to {background:yellow;}
}

```



### 12 svg

```
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
   <circle cx="100" cy="50" r="40" stroke="black" stroke-width="2" fill="red" />
</svg>
```

内置图形

- 矩形 <rect>

- 圆形 <circle>

- 椭圆 <ellipse>

- 线 <line>

- 折线 <polyline>

- 多边形 <polygon>

- 路径 <path>

  ​



### 13瀑布流

我们将要用到的是CSS3新加的column属性，通过指定容器的列个数column-count，列间距column-gap，列中间的边框(间隔边线)column-rule，列宽度column-width实现。



### 14渐进增强和优雅降级

```
.transition { /*渐进增强写法*/
  -webkit-transition: all .5s;
     -moz-transition: all .5s;
       -o-transition: all .5s;
          transition: all .5s;
}
.transition { /*优雅降级写法*/
          transition: all .5s;
       -o-transition: all .5s;
     -moz-transition: all .5s;
  -webkit-transition: all .5s;
}
```

## 15 px、em、rem区别介绍

```
PX:
PX实际上就是像素，用PX设置字体大小时，比较稳定和精确。但是这种方法存在一个问题，当用户在浏览器中浏览我们制作的Web页面时，如果改变了浏览器的缩放，这时会使用我们的Web页面布局被打破。这样对于那些关心自己网站可用性的用户来说，就是一个大问题了。因此，这时就提出了使用“em”来定义Web页面的字体。

EM:
EM就是根据基准来缩放字体的大小。EM实质是一个相对值，而非具体的数值。这种技术需要一个参考点，一般都是以<body>的“font-size”为基准。如WordPress官方主题Twenntytwelve的基准就是14px=1em。
另外，em是相对于父元素的属性而计算的，如果想计算px和em之间的换算，输入数据就可以px和em相互计算。

Rem:
EM是相对于其父元素来设置字体大小的，这样就会存在一个问题，进行任何元素设置，都有可能需要知道他父元素的大小。而Rem是相对于根元素<html>，这样就意味着，我们只需要在根元素确定一个参考值。

浏览器的兼容性
除了IE6-IE8r，其它的浏览器都支持em和rem属性，px是所有浏览器都支持。
因此为了浏览器的兼容性，可“px”和“rem”一起使用，用"px"来实现IE6-8下的效果，然后使用“Rem”来实现代浏览器的效果。
```
## 16 响应式 Web 设计 - Viewport

## 设置 Viewport

一个常用的针对移动网页优化过的页面的 viewport meta 标签大致如下：

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

- width：控制 viewport 的大小，可以指定的一个值，如 600，或者特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
- height：和 width 相对应，指定高度。
- initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。
- maximum-scale：允许用户缩放到的最大比例。
- minimum-scale：允许用户缩放到的最小比例。
- user-scalable：用户是否可以手动缩放。

## 17