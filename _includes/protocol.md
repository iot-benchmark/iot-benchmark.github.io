[//]: # Meant for inclusion from pages under \_protocols, which defines
[//]: # protocols. Shows a description of the protocol and lists associated
[//]: # results in a table.

{% assign protocol = page %}

# Protocol: {{ protocol.name }}

## Description
{{ page.description }}

## Results for {{ protocol.name }}

[//]: # Build a table with one row per profile, one column per environment,
[//]: # and each cell showing all results with results for this protocol.

|  | {% for environment in site.environments %} [{{environment.name}}](/environments/{{environment.env_id}}) | {% endfor %}
| --- | {% for result in site.results %} --- | {% endfor %}
{%- for profile in site.profiles %}
| [{{profile.name}}](/profiles/{{profile.uid}}) |
{%- for environment in site.environments -%}
{%- assign cell_results = site.results | where: "profile", profile.uid | where: "protocol": protocol.uid | where: "environment", environment.env_id -%}
{%- for result in cell_results -%}
{%- assign profile = site.profiles | where: "uid", result.profile | first -%}
[[{{result.configuration}}]](/results/{{result.result_id}})<br />
{%- endfor -%}
 |
{%- endfor -%}
{%- endfor %}
