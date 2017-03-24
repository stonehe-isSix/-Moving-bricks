# HTML中阻止a标签的默认herf的跳转方法。


* 要阻止a标签跳转，可以改变href的值，直接写成“javaScript：void(0)”或者是“javaScript：；”即可阻止跳转；void是一个操作符，void(0)返回undefined，地址不发生跳转，javaScript：；和javaScript:void(0)是一样的。
请注意，当有同学喜欢使用“javaScript：js_method()”形式，这种方法在传递this等参数的时候容易出现问题。并且会导致不必要的触发window.onbeforeunload事件，而且在IE会导致gif动画图片停止播放。
![](https://ooo.0o0.ooo/2017/03/24/58d4d6dd81c81.png)
![](https://ooo.0o0.ooo/2017/03/24/58d4d6dd67f71.png)

* 在调用的事件上，直接return该调用事件，这种方式是默认调用了函数的return false.
![](https://ooo.0o0.ooo/2017/03/24/58d4d6dd40294.png)

* 在调用事件内，加上 return false;或者在注册的事件内加上return false。

![](https://ooo.0o0.ooo/2017/03/24/58d4d6dd7f101.png)

* 网上很常见的代码，#是标签内置的一个方法，用这种方法点击后网页后返回到页面的最顶端所以又有了“##”“#!”等,
尽管解决了返回顶部的问题但仍存在其他缺陷。#在URL格式中是表示页内锚点的意思，也就是说点击这个链接后，页面会跳到当前页面指定的锚点（或者说“书签”）处。
<a href="#">顶端</a>这也是一般页面“回到顶端”功能的实现方法。
![](https://ooo.0o0.ooo/2017/03/24/58d4d6dd39e02.png)

* preventDefault()阻止事件的默认行为但不支持IE，所以在IE中使用returnValue阻止事件默认行为  。
![]https://ooo.0o0.ooo/2017/03/24/58d4d7a50b64c.png)




