# 前端面试之JavaScript基础知识

## 1、DOM和BOM有什么区别？

**DOM**（文档对象模型，Document Object Model）是应用程序编程接口（API）。目的是为了通过编程的方法来操作对应的HTML内容，比如对元素的增加，修改和删除。我们把这个 HTML 看做一个对象树（DOM树），它本身和里面的所有东西比如 `<div></div> `这些标签都看做一个对象，每个对象都叫做一个节点（node），节点可以理解为 DOM 中所有 Object 的父类。

**Document** 就是我们从网站上下载一个网页，通常是HTML，这个HTML文档就是Document对象。也就是DOM树中的根节点。其中包含了document.title,document.domain等属性，document.getElementById和document.getElementsByTagName等方法。

BOM（浏览器对象模型，Browser Object Model）主要是用来访问和操作浏览器窗口。比如弹出新窗口，移动、缩放和关闭浏览器窗口，提供浏览器详细信息的navigator对象，浏览器加载的页面信息的location对象，显示器信息的screen对象等。我们常用的`location.href = "a.html"`就是BOM中的location对象操作页面跳转。

Window也是BOM种提供的一个对象，像常用的弹窗方法`alert()` ，关闭浏览器窗口`close` 等方法都是window对象提供的。

**DOM** 是为了操作文档出现的 API，document 是其的一个对象；
**BOM** 是为了操作浏览器出现的 API，window 是其的一个对象。

另外，JavaScript由3部分组成：**ECMAScript，DOM，BOM**。

##2、为什么`<script>`标签放在页面底部，`<link>`标签放在head中？

script在没有defer和async属性时，执行顺序是从上到下依次解析执行，而浏览器的JavaScript解释器在对JavaScript所有代码进行解析完成之前，页面的处理会暂时停止，因此页面中的其余内容都不会被浏览器加载或显示出来。因此，如果将`script` 放在页面顶部head中，意味着必须等到全部JavaScript代码被下载、解析和执行完成以后，才能开始呈现页面的内容（浏览器在遇到`<body>` 标签才开始显示内容）。对于需要很多JavaScript代码的页面来说，会导致页面出现明显的延迟，所以需要把JavaScript放到`<body>` 元素中页面内容之后。

同样的，由于页面时从上往下依次解析的，因此，如果`<link>`标签放在其他地方，会出现页面加载出来，但是因为样式没有加载而非常难看，影响用户体验，所以一般放在顶部`<head>`中。

## 3、async和defer的区别

**defer属性**：立即下载脚本，但是会延迟执行，直到遇到`</html>` 结束标签后才执行。如有多个script标签设置了defer属性，则按它们出现的先后顺序执行。defer属性只适用于外部脚本。

**async属性**：立即下载脚本，下载完成后就会执行。，所以不保证按顺序执行。async属性同样只适用于外部脚本。

## 4、浏览器的渲染机制

浏览器首先下载html、css、js。 接着解析生成dom tree、rule tree和rendering tree。 再通过layout后渲染页面。

## 5、DOCTYPE的作用和分类

```
<!DOCTYPE> 声明处于 <html> 标签之前。此标签可告知浏览器文档使用哪种 HTML 或 XHTML 规范。在制作网页时都需要定义文档的类型，即doctype。如果不声明网页的文档类型，浏览器在解析的时候会以怪异模式解析网页代码，不同的浏览器下，怪异模式解析的网页效果差别很大，会造成网页布局排版的错位，因此在写html代码前有必要写明文档类型。
```
常用的 DOCTYPE 声明：

##### Html5

```
<!DOCTYPE html>
```

##### HTML 4.01 严格型

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

##### HTML 4.01 过渡型

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
```

##### HTML 4.01 框架集型

该 DTD 等同于 HTML 4.01 Transitional，但允许框架集内容。

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" 
"http://www.w3.org/TR/html4/frameset.dtd">
```

##### XHTML 1.0 严格型

该 DTD 包含所有 HTML 元素和属性，但不包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

##### XHTML 1.0 过渡型

该 DTD 包含所有 HTML 元素和属性，包括展示性的和弃用的元素（比如 font）。不允许框架集（Framesets）。必须以格式正确的 XML 来编写标记。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

##### XHTML 1.0 框架集型

该 DTD 等同于 XHTML 1.0 Transitional，但允许框架集内容。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Frameset//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```

##### XHTML 1.1

该 DTD 等同于 XHTML 1.0 Strict，但允许添加模型（例如提供对东亚语系的 ruby 支持）。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" 
"http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">
```

## 6、ECMAScript有哪些关键字和保留字？

**关键字**（带星号的为第5版新增的关键字）

|   break   |    do    | instanceof | typeof |
| :-------: | :------: | :--------: | :----: |
|   case    |   else   |    new     |  var   |
|   catch   | finally  |   return   |  void  |
| continue  |   for    |   switch   | while  |
| debugger* | function |    this    |  with  |
|  default  |    if    |   throw    |        |
|  delete   |    in    |    try     |        |

**保留字**

| abstract |    enum    |    int    |    short     |
| :------: | :--------: | :-------: | :----------: |
| boolean  |   export   | interface |    static    |
|   byte   |  extends   |   long    |    super     |
|   char   |   final    |  native   | synchronized |
|  class   |   float    |  package  |    throws    |
|  const   |    goto    |  private  |  transient   |
| debugger | implements | protected |   volatile   |
|  double  |   import   |  public   |              |

第 5 版把在非严格模式下运行时的保留字缩减为下列这些:

```
  class          enum          extends         super
  const          export        import
```

在严格模式下，第 5 版还对以下保留字施加了限制:

```
implements	interface	let		package		private		protected
public		static		yield
```

## 7、JavaScript怎样进行垃圾回收？

垃圾回收机制的原理：垃圾收集器会按照固定的时间间隔或代码执行中预定的收集时间，周期性地执行以下操作——找出不再继续使用的变量，然后释放其占用的内存。

1、标记清除

​	JavaScript最常用的垃圾收集方式。当变量进入环境时，这个变量标记为“进入环境”；而当变量离开环境时，则将其标记为“离开环境”。可以使用一个“进入环境”的变量列表及一个“离开环境”的变量列表来跟踪变量的变化，也可以翻转某个特殊的位来记录一个变量何时进入环境及离开环境。

2、引用计数

​	不太常见的垃圾收集策略。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则该值的引用次数就是1；如果同一个值又被赋给另一个变量，则该值的引用次数加1；如果包含对该值引用的变量又取得了另外一个值，则该值的引用次数减1。当该值的引用次数变为0时，则可以回收其占用的内存空间。当垃圾回收器下一次运行时，就会释放那些引用次数为0的值所占用的内存。但是如果是循环引用，a引用b，b引用a，则引用次数永远不会为0，因此不推荐使用引用计数。

而解除引用可以优化内存，真正的作用就是让值脱离执行环境，以便垃圾收集器下次运行时将其回收。

## 8、栈内存和堆内存

栈内存：为编译器自动分配和释放。

堆内存：为成员分配和释放，由程序员申请，自己释放，否则会内存泄漏。

![](https://img-blog.csdn.net/20141212220233511?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGRkMTk5MTA1MDU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

![](https://img-blog.csdn.net/20141213140950906?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGRkMTk5MTA1MDU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

当我们看到一个变量类型是已知的，就分配在栈里面，比如INT,Double等。其他未知的类型，比如自定义的类型，因为系统不知道需要多大，所以程序自己申请，这样就分配在堆里面。

**为什么会有栈内存和堆内存之分？**

​     通常与垃圾回收机制有关。为了使程序运行时占用的内存最小。

​     当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会逐个放入这块栈内存里，随着方法的执行结束，这个方法的内存栈也将自然销毁了。因此，所有在方法中定义的变量都是放在栈内存中的；

​     当我们在程序中创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是堆内存。堆内存中的对象不会随方法的结束而销毁，即使方法结束后，这个对象还可能被另一个引用变量所引用（方法的参数传递时很常见），则这个对象依然不会被销毁，只有当一个对象没有任何引用变量引用它时，系统的垃圾回收机制才会在核实的时候回收它。

