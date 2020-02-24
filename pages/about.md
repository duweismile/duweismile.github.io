---
layout: page
title: About
description: 技术改变世界
keywords: DuDuXiong, 嘟嘟熊
comments: true
menu: 关于
permalink: /about/
---

我是嘟嘟熊，

本博客模板完全从码志(https://mazhuang.org/)fork而来，

由于不喜欢csdn等博满是广告博客，因此利用github的pages搭建次博客

非常感谢码志提供了如此精美的博客系统，博客界面在此基础上无任何改动，真的非常感谢

也欢迎大家去fork

本博客只用来记录自己在学习编程中的一些积累和总结、由于个人水平有限，难免会出现错误，

希望大家见谅，如有问题可联系我的邮箱或qq 谢谢大家
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
