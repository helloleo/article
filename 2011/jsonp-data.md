---

title: 原生 JS 通过 JSONP 获取数据  
category: code  
date: 2011-08-21  

---

之前是使用jQuery来获取JSONP数据，但是对于一些功能简单的小站点，只为了获取JSONP数据就引用一个几十K的库有些不划算，搜寻了一些文章使用原生JS来获取JSONP数据，整理出来。

#### 什么是JSONP？
关于JSONP的详细介绍请移步[维基百科](http://wikipedia.org/wiki/JSONP)。

#### 客户端如何获取JSONP数据？
假设数据URL为：[http://www.example.com/jsonp/](http://www.example.com/jsonp/)，并且该URL支持JSONP调用数据，表明回调函数的键为“callback”(这里的“callback”为服务器端设定，并非标准)

假设我们使用jsonpData()函数来接收处理返回数据，那么请求的URL应该是：http://www.example.com/jsonp?callback=jsonpData 。

假设该URL的数据为{"name":John,"age":24}
返回的数据应该是：jsonpData({"name":John,"age":24})

在请求该数据之前应该先定义好jsonpData()来处理数据;比如：

	var jsonpData = function(){
		var name = jsonpData.name;
		var age= jsonpData.age;
		alert(name+":"+age);
	}

一个完整的例子：

	<script type="text/javascript">
	var jsonpData = function(){
		var name = jsonpData.name;
		var age= jsonpData.age;
		alert(name+":"+age);
	}
	var getJSONP = function(){
		var script = document.createElement("script"); 
		script.src = "http://www.example.com/jsonp?callback=jsonpData"; 
		document.getElementsByTagName("head")[0].appendChild(script); 
	}
	</script>

若是动态的间隔性的获取数据可能会得到缓存中的数据，为了避免这个情况可以在请求地址后面加一个随机数

	"http://www.example.com/jsonp?callback=jsonpData" + "&" + parseInt(Math.random()*10000);

动态请求多次会产生很多script标签，所以每次请求完成后，动态删除掉动态生成的script标签。

#### 一些不足。
最大的不足是没有错误处理，解决办法可以参考：http://www.cnblogs.com/snandy/archive/2011/05/05/2034312.html


#### 参考文章：

Ajax数据载体JSON与JSONP
http://www.nowamagic.net/ajax/ajax_JsonAndJsonp.php

跨域请求之JSONP三
http://www.cnblogs.com/snandy/archive/2011/05/05/2034312.html

