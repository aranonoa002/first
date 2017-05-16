---
title: JavaWeb路径问题总结
date: 2017-05-17 20:17:47
type: "tags"
tags:
  - 001
  - 002
  - 003
---

这几天开发的过程中遇到路径相关的问题，具体的讲包括js和css加载不出来，调试之后发现根本没有调用到，应该是路径的问题；还有一些是前后端交互的一些路径问题，在此做个总结，希望有所帮助。

---

## 相对路径和绝对路径的概念

 - 绝对路径：url地址以“/”开头
 - 相对路径：url地址不以“/”开头

## 根目录

有个项目noa1,项目下有login.html。如果想要访问login页面，则浏览器的地址是：`http://localhost:8080/noa1/login.html`。

根目录常用有以下两种：

 - web站点根目录：`http://localhost:8080/`
 - 应用程序的根目录：`http://localhost:8080/noa1/`

## request.getRequestDispatcher和response.sendRedirect

`request.getRequestDispatcher(url).forward(request, response);`转发，又叫服务器跳转，转发时浏览器的地址讲不会变化，客户端发送请求给服务器，服务器返回一次相应。因为是服务器跳转，**所以根目录是当前应用程序根目录**。
`response.sendRedirect`重定向，又叫浏览器跳转，客户端发送请求给服务器，服务器会相应一个地址给客户端，客户端根据这个地址再次发送请求，服务器做出相应。也就是说浏览器跳转是两个请求，两个相应。由于是浏览器跳转，**所以根目录是web站点的根目录**。

假设noa1项目下有文件夹a、文件夹b，文件夹a下有a.html，文件夹b下有b.html。
从a.html到b.html两种方法：

 - 转发，由于是服务器跳转，根目录是当前项目。所以url不需要带项目名称。相对路径：`../b/b.html`，绝对路径：`/b/b.html`。从a.html转发到b.html时，浏览器地址不变，所以b.html的外部样式引用要按照a.html的路径来。
 - 重定向，由于是浏览器跳转，根目录是web站点。所以url需要带项目名称。相对路径：`../b/b.html`，转发和重定向的相对路径都是一样的，服务器会根据a.html的位置和相对地址找到b.html，在修改相应的地址信息之后发送url给浏览器，浏览器会进行第二次请求。绝对路径：`/noa1/b/b.html`，使用绝对路径时，服务器会根据web站点根目录和绝对路径直接拼接成url地址，发送给浏览器。

## web.xml中的路径问题
 
web.xml中一般是绝对路径
web.xml是描述项目部署的相关信息的，所以web.xml的根目录是当前应用程序的根目录
 
``` xml
 <servlet-mapping>
		<servlet-name>a1</servlet-name>
		<url-pattern>/a1</url-pattern>
</servlet-mapping>
```
 
项目名称：noa1
这个servlet会拦截`http://localhost:8080/noa1/a1`的请求
 
## href=''和action=''的url根目录
 
 `<form action="/xxx"></form>`和`<a href="/xxx"></a>`中的根目录都是web站点的根目录。
 

