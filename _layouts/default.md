<!DOCTYPE html>
<html lang="en">
<head>
{% include head.ext %}
  <title>TGUI: {{ page.title }}</title>
</head>

<body>
  <div id="body" class="NoMargin">
    {% include navigation.ext %}

{% comment %}
    {% capture page_url_without_index_html %}{{ page.url | remove: "/index.html" }}{% endcapture %}
    {% assign splitted_url_parts = page_url_without_index_html | split: '/' %}
    {% capture forLoopMaxInt %}{{ splitted_url_parts.size | minus:1 }}{% endcapture %}
    {% if forLoopMaxInt >= "2" %}
      <ul id="breadcrumbs">
      <li><a href="/">TGUI</a></li>
      {% for i in (1..forLoopMaxInt) %}
        {% capture current_breadcrumb_url %}{{next_prepender}}/{{ splitted_url_parts[i] }}/index.html{% endcapture %}
        {% capture current_breadcrumb_md_url %}{{next_prepender}}/{{ splitted_url_parts[i] }}/{% endcapture %}
        {% capture next_prepender %}{{next_prepender}}/{{ splitted_url_parts[i] }}{% endcapture %}
        {% for breadcrumb_page in site.pages %}
          {% if current_breadcrumb_url == breadcrumb_page.url or current_breadcrumb_md_url == breadcrumb_page.url  %}
            <li>›</li>
            {% if i == forLoopMaxInt %}class="active"{% endif %}
            {% capture breadcrumb_page_page_url_without_index_html %}{{ breadcrumb_page.url | remove: "index.html" }}{% endcapture %}
            <li><a href="{{ site.baseurl }}{{breadcrumb_page_page_url_without_index_html}}">{{breadcrumb_page.breadcrumb}}</a></li>
          {% endif %}
        {% endfor %}
      {% endfor %}
      </ul>
    {% endif %}
{% endcomment %}

    <div id="contents">
      {{ content }}
    </div>

    {% include page_footer.ext %}
  </div>

  {% include footer.ext %}
</body>
</html>
