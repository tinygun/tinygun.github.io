﻿---
layout: page
title: Categories Archive
description: "An archive of posts sorted by category."
permalink: /categories/
comments: false
---
<!--see tag-->
<h4>
    <i class="fa fa-refresh">&nbsp;&nbsp;</i>
    <a href="{{ site.url }}/tags">tag로 찾기</a>
</h4>

{% capture site_categories %}{% for category in site.categories %}{{ category | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign categories_list = site_categories | split:',' | sort %}

<ul class="entry-meta inline-list">
    {% for item in (0..site.categories.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ categories_list[item] | strip_newlines }}{% endcapture %}
    <li><a href="#{{ this_word }}" class="tag"><span class="term">{{ this_word }}</span> <span class="count">{{ site.categories[this_word].size }}</span></a></li>
    {% endunless %}{% endfor %}
</ul>

{% for item in (0..site.categories.size) %}{% unless forloop.last %}
{% capture this_word %}{{ categories_list[item] | strip_newlines }}{% endcapture %}
<article>
<h2 id="{{ this_word }}" class="category-heading">{{ this_word }}
    <small><span class="count">({{ site.categories[this_word].size }})</span></small>
</h2>
<ul>
{% for post in site.categories[this_word] %}
    {% if post.title != null %}
    <li class="entry-title">
        <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}">
        {{ post.title }}&nbsp;<small class="post-date">{{ post.date | date_to_string }}</small>
        </a>
    </li>
    {% endif %}
{% endfor %}
</ul>
</article><!-- /.hentry -->
{% endunless %}{% endfor %}
