### 1、有那些dom接口可以获取一个元素的尺寸（宽度和高度）

假设该元素id为box

```
    var box = document.getElementById('box');
    var w = box.style.width;
    var h = box.style.height;
```

2.通过计算元素的大小（但是在ie情况下有一个问题，那就没写widht和height的css就返回auto，getComputedStyle在IE下如果设置了宽高为em这种单位,返回来的值依然是em,而不会是px）;

```
    var style = window.getComputedStyle ? window.getComputedStyle(box,null) : null || box.currentStyle;
    var w = style.width;
    var h = style.height;
```

3.offsetWidth和offsetHeight

4.getBoundingClientRect（IE67的left、top会少2px,并且没有width、height属性，需兼容该部分浏览器的，那就不得不选择放弃了）DOMRect 对象包含了一组用于描述边框的只读属性——left、top、right和bottom，单位为像素。除了 width 和 height 外的属性都是相对于视口的左上角位置而言的



### 2、请描述dom事件的流程，即从触发到结束的整个过程

事件流包括三个阶段:**事件捕获阶段**、**处于目标阶段**和**事件冒泡阶段。**首先发生的是事件捕获，

为截获事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶 段，可以在这

个阶段对事件做出响应。 单击<div>元素会按照如下图：

![](https://user-gold-cdn.xitu.io/2018/3/31/1627c27a702e7f5e?imageslim)

