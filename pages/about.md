---
layout: page
title: About
description: make the change
keywords: Miko Song
comments: true
menu: 关于
permalink: /about/
---

7年后端开发

先后在爱立信，百度，腾讯，虾皮从事电信核心网，广告检索系统及电商检索系统开发

主要技术栈：rpc 分布式 存储 流式引擎等领域
主要语言：c++，python，java，go
## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
{% if site.url contains 'sinomiko' %}
<li>
微信公众号：<br />
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ site.url }}/assets/images/qrcode_258.jpg" alt="后端技术指北" />
</li>
{% endif %}
</ul>


## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
