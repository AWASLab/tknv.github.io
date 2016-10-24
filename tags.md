---
layout: default
title: tags
permalink: /tags/
tags: tags
---
{% assign _tags = site.tags | sort %}
<div class="tag-cloud">
    {% for _tag in _tags %}
    <span class="site-tag">
        <a href="#{{ _tag | first }}"
            style="font-size: {{ _tag | last | size  |  times: 4 | plus: 80  }}%">
                {{ _tag[0] | replace:'-', ' ' }} ({{ _tag | last | size }})
        </a>
    </span>
    {% endfor %}
</div>

<!-- Get the tag name for every tag on the site and set them
to the `site_tags` variable. -->
{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
<!-- `tag_words` is a sorted array of the tag names. -->
{% assign tag_words = site_tags | split:',' | sort %}
<!-- Posts by Tag -->
<div>
  {% for item in (0..site.tags.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ tag_words[item] }}{% endcapture %}
    <h2 id="{{ this_word }}">{{ this_word }}</h2>
    {% for post in site.tags[this_word] %}{% if post.title != null %}
      <div>
        <span style="float: left;">
          <a href="{{ post.url }}">{{ post.title }}</a>
        </span>
        <span style="float: right;">
          {{ post.date | date_to_string }}
        </span>
      </div>
      <div style="clear: both;"></div>
    {% endif %}{% endfor %}
  {% endunless %}{% endfor %}
</div>
