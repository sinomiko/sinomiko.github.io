---
layout: page
title: About
description: make the change
keywords: Miko Song
comments: true
menu: 关于
permalink: /about/
---

我是miko，来自爱立信中国的小码农

技术改变世界。。。。。。

## I Believe

* 技术没有难点
* 努力改变人生

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}
			
			   
		   
		   

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}