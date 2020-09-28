---
description: >-
  来自
  <https://blog.mindedsecurity.com/2010/09/twitter-domxss-wrong-fix-and-something.html>
---

# 参考资料：A Twitter DomXss, a wrong fix and something more

> WEDNESDAY, SEPTEMBER 22, 2010

##  A Twitter DomXss, a wrong fix and something more              

##   一个推特的DOMXSS，它的一个错误的修复和一些其他的东西 

## 

A Twitter DOM Xss                                            

一个推特的DOM XSS 

It seems that twitter new site, introduced some issue resulting in a worm exploiting a stored Xss. 

推特的新站点似乎引入了一些问题，这些问题导致了可以被蠕虫利用的存储XSS。 

They also added some new JavaScript in their pages which I casually saw while searching in the html for the worm payload. 

他们也在页面中加入了一些新的JAVASCRIPT,我在html中搜索蠕虫的有效荷载时顺便发现了它们 

The code was the following :

 代码如下：

```text
//<![CDATA[ (function(g){var a=location.href.split("#!")[1];if(a){g.location=g.HBR=a;}})(window); //]]>
```

 Do you spot the issue? 

你发现问题出现在哪了吗？ 

It search for "\#!" in the Url and assign the content after that to the window.location object. And it is present in \(almost?\) every page on twitter.com main site.

 这行代码会在url中搜索"\#!"，并将它之后的内容分配给window.location对象。这个问题现在存在twitter.com主站上（几乎？）每个页面上。 

According to DOM Xss Wiki the location object is one of the first objects identified for being dangerous as it is both a source and a sink 

根据DOM xss wiki，location对象是被标记为危险的第一批对象之一，因为它既是source又是sink

 In fact the DOM Based Xss will be triggered by simply going to: 

事实上DOM BASED XSS会被这种简单的方式触发： 

```text
http://twitter.com/#!javascript:alert(document.domain); 
```

as shown in the following screenshot: 

就像下面截图中展示的那样：

![](../../.gitbook/assets/image%20%2871%29.png)

Very simple and effective. After spotting the issue, I sent an email to twitter warning them about it but not without apologizing for finding it in the middle of worm spreading. 

非常简单并有效 在发现这个漏洞后，我给推特发了一封邮件去警告这个问题，但是它们并没有为蠕虫的传播而道歉 。

The response was quite funny because, even if the issue was very straightforward, they cannot reproduce it because of safari antiXss filter. 

这种反应真的非常好笑，因为他们不能在存在antixss过滤器的safari浏览器上复现这个漏洞，即使这个漏洞非常的直接了当。 

Obviously I checked on every browser but Safari ... and guess what ? Safari blocked it! We'll talk about it later. 

很明显我在所有的浏览器上检测了这个漏洞，但是safari，你们猜猜？ safari阻拦了这个漏洞！我们稍后再谈谈这个。 

So I told them that it worked on Firefox, Chrome and Opera and after that they confirmed the issue, thanked me and so long. No more mails. 

我告诉他们这个漏洞在 Firefox, Chrome 和Opera上可以触发之后，他们非常感谢我，并确认了这个漏洞的存在。之后就再也没有消息了。

A Wrong Fix \(To Replace or not to Replace \) 

一个错误的修复（更换或者不更换） 

Thanks Gaz for the title. After some hours, I found the following fix: 

在几个小时后，我发现了这个漏洞的修复 

```text
var c = location.href.split("#!")[1]; if (c) { window.location = c.replace(":", ""); } else { return true; }
```

What's wrong with that? 

它有什么问题？

1. **Data Validation.** 'c' is not validated as directed, that means every character but ":" is allowed. Data validation is about limiting the set of every possible input to an expected subset. Question is do we need to allow everything but one char?

   **数据验证：**'c'没有按照指示进行验证，那就意味着可以使用除了":"以外的任何字符。数据验证就是将每个可能输入集合限制为预期的子集。问题在于我们需要去只限制这一个字符吗？

2. **BlackListing.** It is widely known that blacklisting could lead to bypasses if it is done loosely.

   **黑名单：**众所周知，如果黑名单限制不够严格，就会导致绕过。

3. **No output encoding is applied.** Since location assignment calls the URL Parser the context is quite known and have it's own metacharacters and structure. encoding in the URLParser context is also known as URLEncoding.

   **没有对输出进行编码：**由于位置分配调用了URL解析器，因此上下文是众所周知的，并且具有其自己的元字符和结构。URLParser上下文中的编码也称为URLEncoding。

4. **The use of replace ... let's see the doc \(ECMA Specification\):**

   **使用的替换...让我们看看文档（ECMA 规范）：**                                                                          \[...\] String.prototype.replace \(searchValue, replaceValue\)                                                                      \[...\] If searchValue is not a regular expression, let searchString be ToString\(searchValue\) and search string for the first occurrence of searchString. Let m be 0.                                                  如果searchValue不是正则表达式，就会将searchString为ToString\(searchValue\)，并搜索首次出现searchString的字符串，同时令m为0. \[...\]

The analysis tells that the fix is wrong, and in fact is possible to bypass the replace by doubling the colon ':' char. 

以上分析说明这是个错误的修复，事实上可以用两个':'来进行绕过替换

```text
http://twitter.com/#!javascript::alert(document.domain);
```

See the '::' ? The replace just deletes the first occurrence of ':' so we just add two ':'. It has also the drawback to bypass several client side filters, Safari included. 

看看这'::'? 这里的替换只删除了第一个':'，只要我们使用两个':'就可以绕过替换了 还有一个缺点是绕过了包括safari在内的几个客户端过滤器

So I wrote again to twitter: 

所以我再次给推特写信：

Hey, that is not correct! 

嗨，那个修复不对！ 

```text
(function(g){var a=location.href.split("#!")[1];if(a){g.location=g.HBR=a.replace(":","");}})(window); 
```

will allow: 

这种修复无法抵御以下攻击： 

```text
twitter.com/#!javascript::alert(1) 
```

see the :: ? 

看见那两个':'了吗？

 I'd suggest you to urlencode a 

我建议您对其进行urlencode 

or if it breaks things use a whitelist of allowed chars before going to assign a to the location. 

如果出现问题，请设置允许字符的白名单在将字符分配到loaction之前 

Another fix could be using: 

另一个可行的修复： 

```text
location.pathname=a
```

 Or 或

```text
location.search=a
```

 which at least let you stay on the same domain \(not sure it works on every browser\), but I don't know if it's ok for twitter. 

这种修复让用户保持在相同的domain中（不确定它是否能在所有的浏览器上工作），但是我不知道这对于推特来说是否是可行的。 

It's not a easy task, as usual :\) 

这并不是一个简单的任务，像往常一样 :\) 

Also, please, send me an email when the fix is done, cause I don't want to set a cron job to get when the fix is deployed. ... 

并且请在这个修复起效后给我发邮件，因为我并不想去时不时检查漏洞有没有修复。 …

This morning I found the following fix \(no email from them though\): 

今早我发现了如下的修复（没收到邮件）：

```text
(function(g){ var a=location.href.split("#!")[1]; if(a){ g.location=g.HBR=a.replace(":","","g"); } })(window);
```

Which resolves the multiple colons attack, but, IMHO, it is not really correct because of what I've said previously.

 这种修复解决了多":"的攻击，但是，恕我直言，这并没有真正的解决我先前说明的问题。

The Safari Filter Bypass \(the something more part\) 

safari过滤器的突破（更重要的部分）

As a side consequence of the twitter DOM Xss I found myself playing with the Safari anti Xss filter. 

作为twitter dom xss的附带结果，我发现自己正在使用Safari anti Xss 过滤器存在一些问题。

 It seems that it tries to find a match between the payload used in the assignment to location and the values in the Url in browser location bar. 

似乎它会尝试会在分配给laction的有效荷载与浏览器位置栏中的url中的值之间寻找匹配项。 

After checking a bit in order to understand the behavior of the filter, I figured out that it urldecode the Url and then search for the pattern. 

在经过一系列测试后了解了过滤器的行为，我发现它会对网址进行url编码，然后进行模式搜索 

The problem comes because of that. 

问题就是因此产生的。 

In fact since the + is replaced to a space character, then 

事实上，由于"+"被替换为空字符：

```text
twitter.com?#!javascript:1+alert(1) 
```

 Becomes: 

变成了：

```text
twitter.com?#!javascript:1 alert(1)
```

  which obviously won't ever match the needle:

 显然过滤器不会匹配： "javascript:1+alert\(1\)"

 And there you have the bypass. 

然后你就进行了过滤器的突破。

### **Update \(24/09/2010\)** 

Twitter finally set a working patch to the second wrong fix \(see comments\).

 twitter最终为第二个错误的修复设置了一个可行的补丁（看看注释）。 

```text
(function(g){var a=location.href.split("#!")[1]; if(a){g.location=g.HBR=a.replace(/:/gi,"");}})(window);
```

Still not the best,IMHO, but at least it works...well, until there will be a bypass. 

虽然它不是最好的，有一说一，它还是有用的….唔，直到它被突破。 

Also, since the patch just blocks ':' still remains an arbitrary redirect issue. 

这个补丁仅仅修复了':'带来的问题，仍然遗留了一个可以进行任意重定向的漏洞 

```text
twitter.com#!//attacker.ltd/with/a/page/similar/to/twitterlogin/page
```

### Update \(25/09/2010\)

As it was to be expected, there is a bypass \(already public\) which works on IE8 \(~26% market share\). 

不出所料，有一个基于IE8的绕过（已公开）（约占26%的市场份额（译者注：此数据截至原博客发布））

 I found it yesterday independently by Gareth Heyes and Yusuke Hasegawa and reported to Twitter security team. 

我发现它在昨天被Gareth Heyes和usuke Hasegawa独立找到的，并报告给了twitter安全小组 

The bypass takes advantage of the html entity version of ':' which is : or :. 

这个绕过利用了html中':'的实体符号代码

```text
&#58; 或 &#x3a;
```

 Internet Explorer 8, unlikely other browsers, when finds an entity converts it to its original value when it is assigned to the location object. 

IE8并不像其他浏览器一样，它会把找到的实体在分配给location对象后转换为原始值。 

location="&x\#58;" will let the browser to go to ':' and not to literally "&x58;"

 这将使浏览器转到':'而不是字面意义上的"&x58;" 

So, when the patch tries to replace ':' to an empty value, it won't find it, but the assignment to the location will convert it again to a colon. 

所以，当补丁尝试去替换':'为空时，它将不能识别到冒号。 但是对loacation的分配会将其再次转换为冒号。 

```text
witter.com#!javascript&x58;alert(1)
```

it is still valid \(not in blacklist\). 

这种攻击代码仍然有效（它不在黑名单里面）

 Finally, after writing a new mail to twitter security team, they came with a good defensive patch: 

最后，在写了一篇新的邮件给twitter安全小组后，他们使用了一个很好的防御性补丁： 

```text
(function(g){var a=location.href.split("#!")[1];if(a){window.location.hash = "";g.location.pathname = g.HBR = a;}})(window);
```

As I suggested in my first email. 

就像我在我的第一篇邮件里建议的那样。 

What happens here is that the assignment is performed on the correct attribute \(the pathname\) 

这个方案让分配在正确的属性（路径名）上执行

 so that it is parsed in the right context with no possible bypasses to force a new URI scheme. 

因此可以在正确的上下文中对其进行解析，不会产生暴力绕过新的URI方案的情况。 

Now everything _should_ be really ok... well, if all browsers will behave in the right way! 

现在，一切_应该_一切都很好...好吧，如果所有浏览器的行为都正确！

