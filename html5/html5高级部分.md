# HTML5高级部分

记录html5的高级用法。

[TOC]

## 1. Ajax

Ajax是现代Web应用程序开发的一项关键工具。它让你能向服务器异步发送和接收数据，然后用JavaScript解析。Ajax是Asynchronous JavaScript and XML（异步JavaScript与XML）的缩写。

```
function handleButtonPress(e) {
    var httpRequest = new XMLHttpRequest();
    httpRequest.onreadystatechange = handleResponse;
    httpRequest.open("GET", e.target.innerHTML +  ".html");
    httpRequest.send();
}
```

第一步是创建一个新的`XMLHttpRequest` 对象。与之前在DOM中见过的大多数对象不同，你并非通过浏览器定义的某个全局变量来访问这类对象，而是使用关键词`new` ，就像这样：

```
var httpRequest = new XMLHttpRequest();
```

下一步是给`readystatechange` 事件设置一个事件处理器。这个事件会在请求过程中被多次触发，向你提供事情的进展情况。我会在本章后面讨论这个事件（以及其他由`XMLHttpRequest` 对象定义的事件）。我将`onreadystatechange` 属性的值设为`handleResponse` ，稍后会讨论这个函数：`httpRequest.onreadystatechange = handleResponse;`

现在你可以告诉`XMLHttpRequest` 对象你想要做什么了。使用`open` 方法来指定HTTP方法（在这里是`GET` ）和需要请求的URL：

```
httpRequest.open("GET", e.target.innerHTML +  ".html");
```

> **提示** 我在这里展示的是`open` 方法最简单的形式。你还可以给浏览器提供向服务器发送请求时使用的认证信息，就像这样：`httpRequest.open("GET", e.target.innerHTML + ".html", true, "adam", "secret")` 。最后两个参数是应当发送给服务器的用户名和密码。剩下的那个参数指定了该请求是否应当异步执行。它应该始终被设置为`true` 。
>
> 这个函数的最后一步是调用`send` 方法，就像这样：
>
> ```
> httpRequest.send();
> ```
>
> 我在这个例子里没有向服务器发送任何数据，所以`send` 方法无参数可用。