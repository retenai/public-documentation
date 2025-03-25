{% macro feature_card(icon, title, description, link='', link_text='Ver más') %}
-   :{{ icon }}:{ .lg .middle } **{{ title }}**

    ---

    {{ description }}

    {% if link %}<a href="{{ link }}" class="card-link"></a>{% endif %}
{% endmacro %}

{% macro section_overview(sections) %}
<div class="section-overview" markdown>
{% for section in sections %}
<div class="overview-item" markdown>
:{{ section.icon }}:{ .lg .middle } **[{{ section.title }}]({{ section.link }})**

{{ section.description }}
</div>
{% endfor %}
</div>
{% endmacro %}

{% macro section_cards(title, link='', cards_class='') %}
### {% if link %}[{{ title }}]({{ link }}){% else %}{{ title }}{% endif %} { .section-title }

<div class="grid cards" markdown>
{{ caller() }}
</div>
{% endmacro %}

{% macro feature_highlights(title) %}
## {{ title }}

<div class="feature-highlights" markdown>
{{ caller() }}
</div>
{% endmacro %}

{% macro highlight_item(icon, title, description) %}
<div class="highlight-item" markdown>
:{{ icon }}:{ .lg .middle }

<div class="content" markdown>
**{{ title }}**

{{ description }}
</div>
</div>
{% endmacro %} 