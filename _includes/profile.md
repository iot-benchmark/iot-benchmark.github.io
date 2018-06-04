[//]: # Meant for inclusion from pages under \_profiles, which defines
[//]: # evaluation profiles. Shows a description of the profile, i.e.
[//]: # textual, as well as assigned input parameters, and selected
[//]: # observed and output metrics. Also shows a table summary
[//]: # of all results for this profile. Finally, plots all results.

{% assign profile = page %}

# Profile: {{ profile.name }}

## Description
{{ profile.description }}

## Input Parameters

{% if profile.input-parameters == None %}
None
{% else %}
{% for i in profile.input-parameters %}
{%- assign parameter = site.metrics | where: "uid", i[0] | first -%}
* [{{ parameter.name }}](/metrics/{{i[0]}}): {{ i[1] }}
{% endfor %}
{% endif %}

## Observed Metrics

{% if profile.observed-metrics | size == 0 %}
None
{% else %}
{% for i in profile.observed-metrics %}
{%- assign metric = site.metrics | where: "uid", i | first -%}
* [{{ metric.name }}](/metrics/{{i}})
{% endfor %}
{% endif %}

## Output Metrics

{% if profile.output-metrics == None %}
None
{% else %}
{% for i in profile.output-metrics %}
{%- assign metric = site.metrics | where: "uid", i | first -%}
* [{{ metric.name }}](/metrics/{{i}})
{% endfor %}
{% endif %}

## Results

{% assign profile_results = site.results | where: "profile", profile.uid %}
{% assign profile_results = profile_results | sort: "environment" %}

This profile has results for the following results:

[//]: # Build a table with one row per protocol, one column per environment,
[//]: # and each cell showing all results with results for this profile.

|  | {% for environment in site.environments %} [{{environment.name}}](/environments/{{environment.env_id}}) | {% endfor %}
| --- | {% for result in site.results %} --- | {% endfor %}
{%- for protocol in site.protocols %}
| [{{protocol.name}}](/protocols/{{protocol.uid}}) |
{%- for environment in site.environments -%}
{%- assign cell_results = site.results | where: "profile", profile.uid | where: "protocol": protocol.uid | where: "environment", environment.env_id -%}
{%- for result in cell_results -%}
{%- assign profile = site.profiles | where: "uid", result.profile | first -%}
[[{{result.configuration}}]](/results/{{result.result_id}})<br />
{%- endfor -%}
 |
{%- endfor -%}
{%- endfor %}

The data, with for each result a distribution of per-run means, is shown next.

{% include plotly/header.md %}

{% for m in profile.output-metrics %}
{% assign metric = site.metrics | where: "uid", m | first %}

[//]: # For each metric plot performance of each result as a boxplot-init
[//]: # For each result, we construct an array with one element
[//]: # per run, which contains the mean value over the run.
[//]: # The boxplot will show the distribution across runs in a given result.
[//]: # One x item per result.

[//]: # Start new plot
{% assign plot-id = "summary-"| append: {{metric.uid}} %}
{% include plotly/boxplot-init.md %}

[//]: # For each result, build array that summarizes results for the current metric
{% for result in profile_results %}

{% assign protocol = site.protocols | where: "uid", result.protocol | first %}
{% assign environment = site.environments | where: "uid", result.environment | first %}
{% assign results = site.data.results[{{result.result_id}}]}} %}
{% assign result_name = {{protocol.name}} | append: " [" | append: {{result.configuration}} | append: "]<br />" | append: {{environment.name}}  %}
[//]: # Create an empty array where to store the mean result of each run
{% assign result_means = "" | split: "" %}

{% for run in results %}

[//]: # Compute mean metric for this run, populate array result_means
{% assign data = run[1][metric.uid] %}
{% include functions/mean.md %}
{% assign result_means = result_means | push: return %}

{% endfor %}

[//]: # Add data to boxplot (one new x item)
{% assign plot-ydata = result_means %}
{% include plotly/boxplot-add.md name=result_name %}

{% endfor %}

[//]: # Now, plot the current metric
{% include plotly/boxplot-show.md ylabel=metric.name %}

{% endfor %}
