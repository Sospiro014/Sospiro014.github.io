---
layout: page
title: Archive
---

<section>
  {% if site.posts[0] %}

    {% capture currentyear %}{{ 'now' | date: "%Y" }}{% endcapture %}
    {% capture currentmonth %}{{ 'now' | date: "%m" }}{% endcapture %}
    
    {% capture firstpostyear %}{{ site.posts[0].date | date: '%Y' }}{% endcapture %}
    {% capture firstpostmonth %}{{ site.posts[0].date | date: '%m' }}{% endcapture %}
    
    {% if currentyear == firstpostyear %}
        <h3>This year's posts</h3>
        {% if currentmonth == firstpostmonth %}
            <h4>{{ 'now' | date: "%B" }} Posts</h4>
        {% else %}
            <h4>{{ 'now' | date: "%B" }}</h4>
        {% endif %}
    {% else %}  
        <h3>{{ firstpostyear }}</h3>
    {% endif %}

    {% capture current_year_month %}{{ 'now' | date: "%Y-%m" }}{% endcapture %}
    {% capture post_year_month %}{{ site.posts[0].date | date: '%Y-%m' }}{% endcapture %}
    
    {%for post in site.posts %}
      {% unless post.next %}
        <ul>
      {% else %}
        {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
        {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
        {% capture post_year_month %}{{ post.date | date: '%Y-%m' }}{% endcapture %}
        {% capture next_post_year_month %}{{ post.next.date | date: '%Y-%m' }}{% endcapture %}
        
        {% if year != nyear %}
          </ul>
          <h3>{{ post.date | date: '%Y' }}</h3>
          <ul>
        {% endif %}
        
        {% if post_year_month != next_post_year_month %}
          </ul>
          <h4>{{ post.date | date: "%B" }}</h4>
          <ul>
        {% endif %}
      {% endunless %}
        <li>
          <time>{{ post.date | date:"%d %b" }} - </time>
          <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">
            {{ post.title }}
          </a>
        </li>
    {% endfor %}
    </ul>

{% endif %}
</section>
