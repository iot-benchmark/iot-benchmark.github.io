[//]: # Meant for inclusion from pages under \_environments, which defines
[//]: # environments. Shows a description of the environment and lists associated
[//]: # results in a table.

{% assign environment = page %}

# Environment: {{ environment.name }} ({{environment.category}})

* Category: {{environment.category}}
* [Web site]({{environment.www}})

## Description
{{ environment.description }}

## Results for {{ environment.name }}

[//]: # Build a table with one row per profile, one column per protocol,
[//]: # and each cell showing all setups with results for this environment.

|  | {% for protocol in site.protocols %} [{{protocol.name}}](/protocols/{{protocol.uid}}) | {% endfor %}
| --- | {% for setup in site.setups %} --- | {% endfor %}
{%- for profile in site.profiles %}
| [{{profile.name}}](/profiles/{{profile.uid}}) |
{%- for protocol in site.protocols -%}
{%- assign cell_setups = site.setups | where: "profile", profile.uid | where: "protocol": protocol.uid | where: "environment", environment.env_id -%}
{%- for setup in cell_setups -%}
{%- assign profile = site.profiles | where: "uid", setup.profile | first -%}
[[{{setup.configuration}}]](/setups/{{setup.uid}})<br />
{%- endfor -%}
 |
{%- endfor -%}
{%- endfor %}
