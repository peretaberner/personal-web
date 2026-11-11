---
title: "Resume"
layout: page
permalink: /resume/
---

# Resume

<div class="section-card" markdown="0">
<div class="pi-card">
<img src="{{ site.photo | prepend: '/images/' | relative_url }}" class="pi-photo" alt="{{ site.name }}" width="160" height="160">
<div>
<h2 class="pi-name">{{ site.name }}</h2>
<p style="font-style: italic; color: var(--text-secondary);">{{ site.title }}, {{ site.institution }}</p>
<div class="pi-links">
{% if site.email %}<a href="mailto:{{ site.email }}" class="icon-link" title="Email" aria-label="Email">{% include icon.html name="envelope" %}</a>{% endif %}
{% if site.links.cv and site.links.cv != "" %}<a href="{{ site.links.cv | prepend: '/' | relative_url }}" class="icon-link" title="CV" aria-label="CV">{% include icon.html name="cv" %}</a>{% endif %}
{% if site.links.google_scholar and site.links.google_scholar != "" %}<a href="{{ site.links.google_scholar }}" class="icon-link" title="Google Scholar" aria-label="Google Scholar">{% include icon.html name="google-scholar" %}</a>{% endif %}
{% if site.links.researchgate and site.links.researchgate != "" %}<a href="{{ site.links.researchgate }}" class="icon-link" title="ResearchGate" aria-label="ResearchGate">{% include icon.html name="researchgate" %}</a>{% endif %}
{% if site.links.orcid and site.links.orcid != "" %}<a href="{{ site.links.orcid }}" class="icon-link" title="ORCID" aria-label="ORCID">{% include icon.html name="orcid" %}</a>{% endif %}
{% if site.links.linkedin and site.links.linkedin != "" %}<a href="{{ site.links.linkedin }}" class="icon-link" title="LinkedIn" aria-label="LinkedIn">{% include icon.html name="linkedin" %}</a>{% endif %}
</div>
</div>
</div>
</div>

<div class="section-card">
<h3>Education</h3>
<ul>
{% for item in site.data.pi[0].education %}
<li>{{ item | replace: "-","&#8211;" }}</li>
{% endfor %}
</ul>
</div>

<div class="section-card">
<h3>Research Experience</h3>
<ul>
{% for item in site.data.pi[0].experience %}
<li>{{ item | replace: "-","&#8211;" }}</li>
{% endfor %}
</ul>
</div>

<div class="section-card" markdown="0">
<h3>Workshops &amp; Seminars</h3>
{% for talk in site.data.talks %}{% include talk.html talk=talk %}{% endfor %}
</div>

<div class="section-card">
<h3>Awards &amp; Grants</h3>
<ul>
{% for award in site.data.awards %}
<li>{{ award.name }}</li>
{% endfor %}
</ul>
</div>
