---
layout: post
title: VSCode调试Unity
categories: Unity
---

 用习惯VS后Mono实在是太难用了，所以必须找个替代品，当然就是VSCode了！
 VSCode的Debug面板，左上角点设置的那个齿轮，后面加上这段就行了。
	
	{
		"version":"0.1.0",
		"configurations":[ 
			{
				"name":"Unity",
				"type":"mono",
				"address":"localhost",
				"port":56408
			}
		]
	}
	
 顺便推荐这个插件，安好之后可以把VSCode设置成Unity默认编辑器，彻底代替mono
 [Github](https://github.com/dotBunny/VSCode/)