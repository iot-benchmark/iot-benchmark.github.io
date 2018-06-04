---
title: The IoT-Bench Repository
---

# Home Page

Welcome to the IoT-Bench repository.
This repository defines the framework for profiles, as well as a set of profiles and associated results.
* The IoT-Bench website is at: [www.iotbench.ethz.ch](https://www.iotbench.ethz.ch)
* The source code and raw data for this site is hosted in our [Github Pages repository](https://github.com/iot-benchmark/iot-benchmark.github.io)
* To contribute new profiles or results, see our [Wiki](https://github.com/iot-benchmark/iot-benchmark.github.io/wiki)

## Profile Framework

The general IoT-Bench framework can be found [here](/framework).

## Profiles

We currently have the following profiles:
{% for profile in site.profiles %}
* [{{profile.name}}](/profiles/{{profile.uid}})
{% endfor %}

## Environments

We currently have the following environments:
{% for environment in site.environments %}
* [{{environment.name}} ({{environment.category}})](/environments/{{environment.env_id}})
{% endfor %}

## Protocols

We currently have results for the following protocols:
{% for protocol in site.protocols %}
* [{{protocol.name}}](/protocols/{{protocol.uid}})
{% endfor %}

## Results

A summary of current results is shown below:

[//]: # Show a table one row per profile, one column per environment, and in each
[//]: # cell, the results we have results for.

|  | {% for environment in site.environments %} [{{environment.name}}](/environments/{{environment.env_id}}) | {% endfor %}
| --- | {% for result in site.results %} --- | {% endfor %}
{%- for profile in site.profiles %}
| [{{profile.name}}](/profiles/{{profile.uid}}) |
{%- for environment in site.environments -%}
{%- assign cell_results = site.results | where: "profile", profile.uid | where: "environment", environment.env_id -%}
{%- for result in cell_results -%}
{%- assign protocol = site.protocols | where: "uid", result.protocol | first -%}
{%- assign profile = site.profiles | where: "uid", result.profile | first -%}
<small>[{{protocol.name}} [{{result.configuration}}]](/results/{{result.result_id}})</small><br />
{%- endfor -%}
 |
{%- endfor -%}
{%- endfor %}
