<!-- The following part extracts all the tags from your posts and sort tags, so that you do not need to manually collect your tags to a place. -->
{% assign rawtags = "" %}
{% for post in site.posts %}
	{% assign ttags = post.tags | join:'|' | append:'|' %}
	{% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}

<!-- The following part removes dulpicated tags and invalid tags like blank tag. -->
{% assign tags = "" %}
{% for tag in rawtags %}
	{% if tag != "" %}
		{% if tags == "" %}
			{% assign tags = tag | split:'|' %}
		{% endif %}
		{% unless tags contains tag %}
			{% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
		{% endunless %}
	{% endif %}
{% endfor %}

{% for tag in tags %}
<h4 id="{{ tag | slugify }}">{{ tag }}</h4>
<ul>
{% assign navigation_pages = site.posts | sort: 'navigation_weight' %}
{% for post in navigation_pages %}
 {% if post.tags contains tag %}
    <li><a href="/sponsor-guide/{{ post.url }}">{{ post.title }}</a></li>
 {% endif %}
{% endfor %}
</ul>
{% endfor %}