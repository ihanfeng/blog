title: "通过cookie编码转换解决cookie中文问题"
id: 980
date: 2013-10-28 10:15:35
tags: 
- cookie
- cookie中文
categories: 
- JAVA
---

JAVA 代码向客户端增加Cookie信息

{%codeblock lang:java%}
<%
Cookie c1 = new Cookie("name","张三");
Cookie c2 = new Cookie("address","福建省福州市仓山区");
response.addCookie(c1);
response.addCookie(c2);
%>
{%endcodeblock%}

如果变量中带有中文，那么会出现如下异常。

异常信息：`java.lang.IllegalArgumentException: Control character in cookie value, consider BASE64 encoding your value`
<!-- more -->

经过google发现：

cookies只支持ASCII字符，而且不能有逗号，分号，空白。或者以$开头。名字在创建后不能改变。如果要存储中文的，先用URLEcode编码，在存入，取出的时候，用decode解码。

解决方法：

将中文编码改成UTF-8编码格式,传到前台

{%codeblock lang:java%}
String name= URLEncoder.encode("张三", "UTF-8");
String address= URLEncoder.encode("福建省福州市", "UTF-8");
Cookie c1 = new Cookie("name",name);
Cookie c2 = new Cookie("address",address);
response.addCookie(c1);
response.addCookie(c2);
{%endcodeblock%}

那么获取cookie时如何解码：


{%codeblock lang:jsp%}
<%
Cookie c[] = request.getCookies();
for(int i=0;i<c.length;i++){
String name = URLDecoder.decode(c[i].getName(), "UTF-8");
String address = URLDecoder.decode(c[i].getValue(), "UTF-8");
%>
<h3><%=name %>==><%=address%></h3>
<%
}
%>
{%endcodeblock%}