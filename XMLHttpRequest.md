# XMLHttpRequest

## **目录**
* [概述](#_1)
* [Ajax和XMLHttpRequest](#_2)
	+ [什么是Ajax?](#_2_1)
	+ [什么是XMLHttpRequest](#_2_2)
* [细说XMLHttpRequest如何使用](#_3)
* [设置request header](#_4)
* [如何获取request header](#_5)
* [如何指定xhr.respnse的数据类型](#_6)
* [如何获取resopnse数据](#_7)
* [如何追踪ajax请求的当前状态](#_8)

#### **概述**

>看到标题的时候，可能很多的同学说，我已经使用xhr发送很多的ajax请求了，对于它的基本操作已经算熟练了，但是你真的会使用XMLHttpRequest吗？

#### **Ajax和XMLHttpRequest**

>通常我们认为Ajax是等同XMLHttpRequest,但是细究起来他们是两个不同的概念。

1. [什么是Ajax?](http://www.cnblogs.com/SanMaoSpace/archive/2013/06/15/3137180.html)


2. [什么是XMLHttpRequest?](http://blog.csdn.net/liujiahan629629/article/details/17126727)

	通过对Ajax和对XMLHttpRequest的解读，我们了解到，ajax是一种技术方案但是不是一种新的技术方案。他依赖的是css/html/javascript。而其中最核心的依赖是浏览器提供的<font >XMLHttpRequst</font>对象,是这个对象可以发出和接受HTTP响应。

	所以使用一句话来总结两者的关系：我们使用XMLHttpRequest对象来发送一个Ajax请求。

#### **XMLHttpRequest的发展历程**

XMLHttpRequest一开始只是微软浏览器提供的一个接口，后来各大浏览器纷纷效仿也提供了这个接口，随着发展，XMLHttpReques已经被W3C列为标准，XMLHttpReqauest有两个版本，LEVEL1和LEVEL2.

1. XMLHttpRequest Level 1 缺点。
	* 不能发送跨域请求，搜同源策略限制。
	* 不能发送二进制文件（视频，音频，图片），只能发送纯的文本。
	* 在发送和接受请求的时候，不能实时获取进度，只能判读是否已完成。
2. XMLHttpRequest Level 2 新增功能
	* 可以发送跨域文件，在服务器端允许的情况下。
	* 支持发送和接受二进制文件。
	* 新增formData对象，支持发送表单数据。
	* 发送和接受的数据，可以实时的获取进度。
	* 可以设置超时。
>更详细的比较请参考 [http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)

#### **XMLHttpRequest的兼容性**

![兼容性](https://ooo.0o0.ooo/2017/03/21/58d099bcb5636.png)

从图中可以看出：

* IE8/IE9、Opera Mini 完全不支持xhr对象
* IE10/IE11部分支持，不支持 xhr.responseType为json
* 部分浏览器不支持设置请求超时，即无法使用xhr.timeout
* 部分浏览器不支持xhr.responseType为blob

>更多关于兼容性，大家可以浏览"Can I use"网站。

#### **细说XMLHttpRequest如何使用**

先来看一段使用XMLHttpRequest发送Ajax请求的简单示例代码。

				function sendAjax() {
				  //构造表单数据
				  var formData = new FormData();
				  formData.append('username', 'johndoe');
				  formData.append('id', 123456);
				  //创建xhr对象 
				  var xhr = new XMLHttpRequest();
				  //设置xhr请求的超时时间
				  xhr.timeout = 3000;
				  //设置响应返回的数据格式
				  xhr.responseType = "text";
				  //创建一个 post 请求，采用异步
				  xhr.open('POST', '/server', true);
				  //注册相关事件回调处理函数
				  xhr.onload = function(e) { 
				    if(this.status == 200||this.status == 304){
				        alert(this.responseText);
				    }
				  };
				  xhr.ontimeout = function(e) { ... };
				  xhr.onerror = function(e) { ... };
				  xhr.upload.onprogress = function(e) { ... };
				  
				  //发送数据
				  xhr.send(formData);
				}

#### **设置request header**

在发送Ajax请求（实质是一个http请求）时，我们可能可能需要设置请求头部信息，比如conteny-type，connect，cookie，accept-xxx等。xhr提供了setRequestHeader方法来允许我们修改header。

void setRequestHeader(DOMString header,DOMString value);

>注意：

* 方法的第一个参数 header 大小写不敏感，即可以写成content-type，也可以写成Content-Type，甚至写成content-Type;
* Content-Type的默认值与具体发送的数据类型有关，请参考本文【可以发送什么类型的数据】一节；
* **setRequestHeader必须在open()方法之后，send()方法之前调用，否则会抛错；**
* setRequestHeader可以调用多次，最终的值不会采用覆盖override的方式，而是采用追加append的方式。下面是一个示例代码：

				var client = new XMLHttpRequest();
				client.open('GET', 'demo.cgi');
				client.setRequestHeader('X-Test', 'one');
				client.setRequestHeader('X-Test', 'two');
				// 最终request header中"X-Test"为: one, two
				client.send();

#### **如何获取request header**

xhr提供两个方法来回去响应头，getAllResponstHeaders(),getResponseHeader(DOMString header);

>这两个方法看起来简单，但是处处是坑。

1. 使用getAllResponseHeaders()。获取到header与控制台中看到的header不一样。
2. getResponseHeader()获取某个header值得时候，浏览器抛出错误

1. 原因1：[w3c的xhr标准中做了限制](https://www.w3.org/TR/XMLHttpRequest/)，规定在客服端无法获取response中的set-cokie、set-cookie2这连个字段，同域还是不同域请求下。
2. 原因2：[w3c的cors标准对于跨域请求也做了限制](https://www.w3.org/TR/cors/#access-control-allow-credentials-response-header),规定对于跨域请求，客服端允许获取response header字段只限于“simple resopnse header”和“Access-control-expose-headers”;

		"simple response header"包括的 header 字段有：Cache-Control,Content-Language,Content-Type,Expires,Last-Modified,Pragma;
		"Access-Control-Expose-Headers"：首先得注意是"Access-Control-Expose-Headers"进行跨域请求时响应头部中的一个字段，对于同域请求，响应头部是没有这个字段的。这个字段中列举的 header 字段就是服务器允许暴露给客户端访问的字段。

		所以getAllResponseHeaders()只能拿到限制以外（即被视为safe）的header字段，而不是全部字段；而调用getResponseHeader(header)方法时，header参数必须是限制以外的header字段，否则调用就会报Refused to get unsafe header的错误。


#### **如何指定xhr.respnse的数据类型**

有些时候我们希望xhr.response返回的就是我们想要的数据类型。比如：响应返回的数据是纯JSON字符串，但我们期望最终通过xhr.response拿到的直接就是一个 js 对象，我们该怎么实现呢？
有2种方法可以实现，一个是level 1就提供的overrideMimeType()方法，另一个是level 2才提供的xhr.responseType属性。

>xhr.overrideMimeType()

overrideMimeType是xhr level 1就有的方法，所以浏览器兼容性良好。这个方法的作用就是用来重写response的content-type，这样做有什么意义呢？比如：server 端给客户端返回了一份document或者是 xml文档，我们希望最终通过xhr.response拿到的就是一个DOM对象，那么就可以用xhr.overrideMimeType('text/xml; charset = utf-8')来实现。

再举一个使用场景，我们都知道xhr level 1不支持直接传输blob二进制数据，那如果真要传输 blob 该怎么办呢？当时就是利用overrideMimeType方法来解决这个问题的。

			var xhr = new XMLHttpRequest();
			//向 server 端获取一张图片
			xhr.open('GET', '/path/to/image.png', true);
			
			// 这行是关键！
			//将响应数据按照纯文本格式来解析，字符集替换为用户自己定义的字符集
			xhr.overrideMimeType('text/plain; charset=x-user-defined');
			
			xhr.onreadystatechange = function(e) {
			  if (this.readyState == 4 && this.status == 200) {
			    //通过 responseText 来获取图片文件对应的二进制字符串
			    var binStr = this.responseText;
			    //然后自己再想方法将逐个字节还原为二进制数据
			    for (var i = 0, len = binStr.length; i < len; ++i) {
			      var c = binStr.charCodeAt(i);
			      //String.fromCharCode(c & 0xff);
			      var byte = c & 0xff; 
			    }
			  }
			};
			
			xhr.send();
代码示例中xhr请求的是一张图片，通过将 response 的 content-type 改为'text/plain; charset=x-user-defined'，使得 xhr 以纯文本格式来解析接收到的blob 数据，最终用户通过this.responseText拿到的就是图片文件对应的二进制字符串，最后再将其转换为 blob 数据。

>xhr.responseType

responseType是xhr level 2新增的属性，用来指定xhr.response的数据类型，目前还存在些兼容性问题，可以参考本文的【XMLHttpRequest的兼容性】这一小节。那么responseType可以设置为哪些格式呢，我简单做了一个表，如下：

![](https://ooo.0o0.ooo/2017/03/21/58d13f03ae608.png)

下面是同样是获取一张图片的代码示例，相比xhr.overrideMimeType,用xhr.response来实现简单得多。

			var xhr = new XMLHttpRequest();
			xhr.open('GET', '/path/to/image.png', true);
			//可以将`xhr.responseType`设置为`"blob"`也可以设置为`" arrayBuffer"`
			//xhr.responseType = 'arrayBuffer';
			xhr.responseType = 'blob';
			
			xhr.onload = function(e) {
			  if (this.status == 200) {
			    var blob = this.response;
			    ...
			  }
			};
			
			xhr.send();

>小结

虽然在xhr level 2中，2者是共同存在的。但其实不难发现，xhr.responseType就是用来取代xhr.overrideMimeType()的，xhr.responseType功能强大的多，xhr.overrideMimeType()能做到的xhr.responseType都能做到。所以我们现在完全可以摒弃使用xhr.overrideMimeType()了。

#### **如何获取resopnse数据**

>xhr提供了3个属性来获取请求返回的数据，分别是：xhr.response、xhr.responseText、xhr.responseXML

* xhr.response
  + 默认值：空字符串""
  + 当请求完成时，此属性才有正确的值
  + 请求未完成时，此属性的值可能是""或者 null，具体与 xhr.responseType有关：当responseType为""或"text"时，值为""；responseType为其他值时，值为 null
* xhr.responseText
  + 默认值为空字符串""
  + 只有当 responseType 为"text"、""时，xhr对象上才有此属性，此时才能调用xhr.responseText，否则抛错
  + 只有当请求成功时，才能拿到正确值。以下2种情况下值都为空字符串""：请求未完成、请求失败
* xhr.responseXML
  + 默认值为 null
  + 只有当 responseType 为"text"、""、"document"时，xhr对象上才有此属性，此时才能调用xhr.responseXML，否则抛错
  + 只有当请求成功且返回数据被正确解析时，才能拿到正确值。以下3种情况下值都为null：请求未完成、请求失败、请求成功但返回数据无法被正确解析时

#### **如何追踪ajax请求的当前状态**

在发一个ajax请求后，如果想追踪请求当前处于哪种状态，该怎么做呢？

用xhr.readyState这个属性即可追踪到。这个属性是只读属性，总共有5种可能值，分别对应xhr不同的不同阶段。每次xhr.readyState的值发生变化时，都会触发xhr.onreadystatechange事件，我们可以在这个事件中进行相关状态判断

		 xhr.onreadystatechange = function () {
		    switch(xhr.readyState){
		      case 1://OPENED
		        //do something
		            break;
		      case 2://HEADERS_RECEIVED
		        //do something
		        break;
		      case 3://LOADING
		        //do something
		        break;
		      case 4://DONE
		        //do something
		        break;
		    }

![](https://ooo.0o0.ooo/2017/03/21/58d1411ad3da5.png)
