{% macro feature_card(icon, title, description, link='', link_text='Ver más') %}
-   :{{ icon }}:{ .lg .middle } **{% if link %}[{{ title }}]({{ link }}){% else %}{{ title }}{% endif %}**

    ---

    {{ description }}
    {% if link %}
    [:octicons-arrow-right-24: {{ link_text }}]({{ link }})
    {% endif %}
{% endmacro %}

{% macro section_cards(title, link='', cards_class='') %}
### {% if link %}[{{ title }}]({{ link }}){% else %}{{ title }}{% endif %} { .section-title }

<div class="grid cards" markdown>
{{ caller() }}
</div>
{% endmacro %} 