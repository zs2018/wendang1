## 1、什么是JSON？与JavaScript对应数据有什么不同？

JSON是一种数据格式，不是一种编程语言。JSON 使用 JavaScript 语法的子集表示对象、数组、字符串、数值、布尔值和 null。

JavaScript 字符串与 JSON 字符串的最大区别在于，JSON 字符串必须使用**双引号**(单引号会导致语

法错误)。

与 JavaScript 的对象字面量相比，JSON 对象有两个地方不一样。首先，**没有声明变量**(JSON 中没
有变量的概念)。其次，**没有末尾的分号**(因为这不是 JavaScript 语句，所以不需要分号)。

JSON 数组采用的就是 JavaScript 中的数组字面量形式，但同样没有变量和分号。



## 2、怎样对JSON进行解析和序列化？

JSON 对象有两个方法:**stringify()**和 **parse()**。分别把JavaScript 对象序列化为 JSON 字符串和把 JSON 字符串解析为原生 JavaScript 值。

**JSON.stringify()**除了要序列化的 JavaScript 对象外，还可以接收另外两个参数，这两个参数用于指定以不同的方式序列化 JavaScript 对象。第一个参数是个过滤器，可以是一个数组，也可以是一个函数;第二个参数是一个选项，表示是否在 JSON 字符串中保留缩进。

**JSON.parse()**方法也可以接收另一个参数，该参数是一个函数，将在每个键值对儿上调用。


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


​			
​		
​				
​		
​	