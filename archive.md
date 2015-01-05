---
layout: page
title: Archive
permalink: /archive/
---

<div class="archive">
	<ul class="post-list">
	{% for post in site.posts %}
	  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
	  {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
	    {% if year != nyear %}
	      {% if forloop.index != 1 %}</ul>{% endif %}
	      <h3 class="sub-header">{{ post.date | date: '%Y' }}</h3><ul>
	    {% endif %}
	  <li><span class="time">{{ post.date | date: "%B %-d" }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
	{% endfor %}

  </ul>

</div>