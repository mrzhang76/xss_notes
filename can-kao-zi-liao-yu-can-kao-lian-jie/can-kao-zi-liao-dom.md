# 参考资料：DOM

### **什么是DOM？** 

DOM是一个树状模型，可以使用JAVASCRIPT代码根据一层层节点来遍历/获取/修改对应的节点，对象，值。

![](../.gitbook/assets/image%20%2878%29.png)

### **常用的DOM方法** 

用户可以使用JAVASCRIPT或其他的编程语言对HTML DOM进行访问。所有的HTML元素被定义为对象，而编程接口则是对象方法和对象属性。

![](../.gitbook/assets/image%20%2814%29.png)

### **四个重要的DOM属性** 

NodeName属性：规定节点的名称

* 属性只读 
*  元素节点的NodeName与标签名相同 
* 属性节点的NodeName与属性名相同 
* 文本节点的NodeName始终是\#text 
* 文档节点的NodeName始终是\#document

NodeValue属性：规定节点的值 

* 元素节点的NodeValue是undefined或null 
* 属性节点的NodeValue是属性值 
* 文本节点的NodeValue是文本本身

NodeType属性：返回节点的类型

* 只读

innerHTML属性：获取元素内容 

* 可赋值 
* 可读

![txt&#x83B7;&#x53D6;&#x5230;&#x7684;&#x503C;&#x4E3A;&#xFF1A;Hello World!](../.gitbook/assets/image%20%2869%29.png)



### **获取输入** 

JS调用DOM内置对象location来获取用户输入，例如使用location.href来获取或设置当前URL

### **location对象属性**

![](../.gitbook/assets/image%20%2849%29.png)

### 参考链接

[https://www.w3school.com.cn/htmldom/dom\_intro.asp](https://www.w3school.com.cn/htmldom/dom_intro.asp)

[http://blog.nsfocus.net/xss-advance/](http://blog.nsfocus.net/xss-advance/)

