---
uid: somepattern-random
name: A given pattern, 1--180s
description: >
  This profile is *just a placeholder*.
input-parameters:
  traffic-pattern: Some pattern (space domain)
  traffic-period: 1s--180s
  traffic-payload: 4B-1kB
observed-metrics: []
output-metrics: [ e2e-pdr, e2e-latency, power ]
---

{% include profile.md %}
