---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---
<div class="home">
  {%- if page.title -%}
    <h1 class="page-heading">{{ page.title }}</h1>
  {%- endif -%}
  {%- if site.posts.size > 0 -%}
    <h2 class="post-list-heading">{{ page.list_title | default: "Posts" }}</h2>
    <ul class="post-list">
      {%- for post in site.posts -%}
      <li>
	  <br/>
        {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
        <span class="post-meta">{{ post.date | date: date_format }}</span>
		{% if post.image %}
			<img src="{{post.image}}" class="right" width="75px">
		{% endif %}
        <h3>
          <a class="post-link" href="{{ post.url | relative_url }}">
            {{ post.title | escape }}
          </a>
        </h3>
		<span class="post-description">{{ post.description}}</span>
        {%- if site.show_excerpts -%}
          {{ post.excerpt }}
        {%- endif -%}
		<hr/>
      </li>
      {%- endfor -%}
    </ul>

    <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | relative_url }}">via RSS</a></p>
  {%- endif -%}

</div>