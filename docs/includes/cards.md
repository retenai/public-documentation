{% macro feature_card(icon, title, description, link='') %}
-   :{{ icon }}:{ .lg .middle } **[{{ title }}]({{ link }})** {% if link %}[]({{ link }}){.card-link}{% endif %}

    ---

    {{ description }}
{% endmacro %}

{% macro section_overview(sections) %}
<div class="section-overview" markdown>
{% for section in sections %}
<div class="overview-item" markdown>
:{{ section.icon }}:{ .lg .middle } **[{{ section.title }}]({{ section.link }})** []({{ section.link }}){.card-link}

{{ section.description }}
</div>
{% endfor %}
</div>
{% endmacro %}

{% macro section_cards(title='', link='', cards_class='') %}
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