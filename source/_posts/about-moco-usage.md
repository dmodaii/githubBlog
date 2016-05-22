title: 使用moco模拟http(s)请求
toc: true
tags:
  - iOS
  - Web
categories: 第三方库
date: 2016-03-22 13:17:25
---

做为一个前端开发或者移动端开发者，我们可能有时候有这样的需求**想要模拟http请求后台，然后返回一些数据，根据服务器端返回的这些数据做出相对应的解析**，那么使用github上的一个开源库Moco就能解决我们的这个需求。

<!--more-->

``Moco``github地址  
https://github.com/dreamhead/moco

首先去``github``上下载moco的release版本，``zip``或者``tar.gz``格式的都可以

解压出来是一个``.jar``文件，随便命名个名字，我这里是``moco.jar``，然后建立一个文件夹把``moco.jar``复制进去，在根目录建立``index.json``文件，里面填写

### 模拟服务器返回文本

```json
[
  {
    "response" :
      {
        "text" : "Hello, World"
      }
  }
]
```

然后在命令行cd到你这个文件夹，执行

```
java -jar moco.jar http -p 8888 -c index.json
```

停止服务器``ctrl+c``，下面这些配置建议大家配置后看下终端有没有报错，有就先停止服务再启动

现在打开浏览器访问``http://localhost:8888``你就能看到经典的``Hello, World``了。

### 模拟服务器返回json

``moco``也能模拟服务器返回json数据,将index.json文件改为

```json
[
    {"response" : 
    	{"json" :
    		 { "name" : "moexcept"}
    	}
    }
]
```
这是刷新浏览器服务器端返回``{"name":"moexcept"}``;

### 模拟数组，数组里是json

``moco``还能模拟服务器返回数组，将index.json文件改为

```json
[

    {"response" : 
    	{"json" :
    	[
    		 { "name1" : "moexcept"}, {"name2" : "wanghua"}
    		
    	]
    	}
    }
]
```
刷新浏览器返回``[{"name1":"moexcept"},{"name2":"wanghua"}]``

### 返回文件
``moco``也支持返回文件，能够返回文件，我们就能够返回很多种数据，只要在文件中写好就行了。如果闲上面的写法太累，只需要把返回内容写在本目录一个文件，返回这个文件就可以了。

例如我在本地建立一个``test.json``文件

```json
{
    "status" : 0,
    "data" : 
    [
        {"revocation":1}
    ]
}
```

然后在``index.json``文件中写

```josn
[
    {"response" : {"file" : "test.json"}}
]
```

然后刷新服务器，就会返回``test.json``文件中的内容，以后再``test.json``写xml也可以

### 模拟请求路径

说了说返回，再来说说怎么模拟请求路径吧。很简单，配置``request``就可以了

```json
[
	{
 	"request" :
	    {
	      "uri" : "/test"
	    },
   "response" : 
	   {
	   	"file" : "test.json"
	   }
	}
]
```
访问``http://localhost:8888/test``就会看到``test.jso``里面的内容了。

### 设置请求参数
``moco``也可以模拟请求http的请求参数

在``index.json``中配置

```json
[
	{
	 "request" :
	    {
	      "uri" : "/test",
	      "queries" : 
	        {
	          "name" : "moexcept"
	        }
	    },
   "response" : 
	   {
	   	"file" : "test.json"
	   }
	}
]
```

浏览器输入``http://localhost:8888/test?name=moexcept``回车就会出现``test.json``的内容了。

### 包含json文件

``moco``也支持一个``json``文件中请求另外一个``json``文件

将``index.json``文件改为

```json
[
    {"context" : "/test", "include" : "test.json"}
]
```

然后``test.json``文件为

```json
[ 
    { "request" : { "uri" : "/getStatus"}, "response" : { "json" : [{ "status" : "true" }]}},
]
```

注意``这个时候要使用的启动服务的命令也不一样了``

```
java -jar moco.jar http -p 8888 -g index.json
```

浏览器输入``http://8888/test/getStatus``就会看到返回的json数据。

写这片文章也就是介绍一下这个工具的初步用法，具体很复杂的用法请大家参考他的``api``文件,另外moco也支持模拟https请求  
https://github.com/dreamhead/moco/blob/master/moco-doc/apis.md

感谢作者提供这么好的工具。

请多多关注我的博客，我会多多分享自己的学习总结  
http://www.codertian.com

