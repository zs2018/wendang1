## 1、BOM提供了哪些对象？

window对象：

```
BOM 的核心对象是 window，它表示浏览器的一个实例。在浏览器中，window 对象有双重角色， 它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象。
```

location对象：

```
它提供了与当前窗口中加载的文档有关的信息，还提供了一 些导航功能。事实上，location 对象是很特别的一个对象，因为它既是 window 对象的属性，也是 3 document 对象的属性;
换句话说，window.location 和 document.location 引用的是同一个对象。
```

navigator对象：

```
已经成为识别客户端浏览器的事实标准。每个浏览器中的 navigator 对象也都有一套自己的属性。
```

screen对象：

```
screen 对象在编程中用处不大，只用来表明客户端的能力，其中包括浏览器窗口外部的显示器的信息，如像素宽度和高度等。
```

history对象：

```
history 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。因为 history 是 window 对象的属性，因此每个浏览器窗口、每个标签页乃至每个框架，都有自己的 history 对象与特定的 window 对象关联。
```



## 2、setTimeout和setInterval

setTimeout() 它接受两个参数:要执行的代码和以毫秒表示的时间(即在执行代码前需要等待多少毫秒)。

```
JavaScript 是一个单线程序的解释器，因此一定时间内只能执行一段代码。为了控制要执行的代码，就有一个 JavaScript 任务队列。这些任务会按照将它们添加到队列的顺序执行。
```

注意：超时调用的代码都是在全局作用域中执行的，因此函数中 this 的值在非**严格模式下指向 window 对象**，在**严格模式下是 undefined**。

setInterval() 接受参数分别是要执行的代码(字符串或函数)和每次执行之前需要等待的毫秒数。



## 3、querySelector和querySelectorAll

querySelector()方法接收一个 CSS 选择符，返回与该模式匹配的第一个元素，如果没有找到匹

配的元素，返回 null。

querySelectorAll()方法接收的参数与 querySelector()方法一样，都是一个 CSS 选择符，但返回的是所有匹配的元素而不仅仅是一个元素。这个方法返回的是一个 NodeList 的实例。


​			
​		
​	




​			
​		
​	



​	




​			
​		
​	


​			
​		
​	




​			
​		
​		
​		
​	