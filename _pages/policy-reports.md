---
title: "Policy Reports"
layout: page
permalink: /policy-reports/
---

# Policy Reports

{% comment %}To add a report, edit _data/policy_reports.yml{% endcomment %}
Evaluations and studies carried out at [KSNET](https://www.ksnet.eu) for public administrations and the third sector.

<input type="text" class="pub-search" id="pubSearch" placeholder="Filter by title, client, or year...">

{% assign groups = site.data.policy_reports | group_by: "year" | sort: "name" | reverse %}
<div class="section-card" id="pubList" markdown="0">
{% for g in groups %}
<h2>{{ g.name }}</h2>
{% for r in g.items %}
{% capture rid %}rep-{{ g.name }}-{{ forloop.index }}{% endcapture %}
<div class="pub-entry report-entry" data-pub-searchable>
  <div class="text-justify"><strong>{{ r.title }}</strong>{% if r.title_local %}<br><em class="report-local">{{ r.title_local }}</em>{% endif %}</div>
  <div class="report-meta">
    {% if r.client %}<span><b>Client:</b> {{ r.client }}</span>{% endif %}
    {% if r.role %}<span><b>Role:</b> {{ r.role }}</span>{% endif %}
    {% if r.language %}<span><b>Language:</b> {{ r.language }}</span>{% endif %}
  </div>
  {% if r.links or r.summary %}
  <div class="pub-actions">
    {% for l in r.links %}<a href="{{ l.url }}" target="_blank" class="btn-pill btn-link">{{ l.label }}</a>{% endfor %}
    {% if r.summary %}<button class="btn-pill btn-abstract" data-toggle-target="{{ rid }}">Summary</button>{% endif %}
  </div>
  {% endif %}
  {% if r.summary %}
  <div class="pub-collapse" id="{{ rid }}"><div class="report-summary">{{ r.summary | markdownify }}</div></div>
  {% endif %}
</div>
{% endfor %}
{% endfor %}
</div>
