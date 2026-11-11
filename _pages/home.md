---
title: "Home"
layout: homelay
permalink: /
---

<h1 class="home-hero">{{ site.name }}</h1>
<p class="home-hero-sub">{{ site.title }}, {{ site.institution }}</p>

<div class="chip-container" markdown="0">
<a href="{{ '/research/' | relative_url }}" class="chip">Economics of Education</a>
<a href="{{ '/research/' | relative_url }}" class="chip">Labour Economics</a>
<a href="{{ '/research/' | relative_url }}" class="chip">Gender Economics</a>
<a href="{{ '/research/' | relative_url }}" class="chip">Applied Microeconometrics</a>
<a href="{{ '/research/' | relative_url }}" class="chip">Data Science</a>
<a href="{{ '/policy-reports/' | relative_url }}" class="chip">Policy Evaluation</a>
</div>

I am a **Senior Research Economist** and **Research Coordinator** at [KSNET](http://www.ksnet.eu). We are dedicated to knowledge transfer, we make academic research in economics accessible to society, firms, public administration, and the third sector.

I am a **PhD Candidate in Economics** at the [University of Barcelona School of Economics](https://www.ub.edu/school-economics/) and at the [Public Policy Program at Institut d'Economia de Barcelona (IEB)](http://ieb.ub.edu/inicio/politicas-publicas/). My doctoral thesis consists of analyzing external factors that affect university performance from a gender perspective. The three factors I analyze are pressure, crime around student housing, and the professor’s gender.

I am also the **Academic Secretary** of the [*Journal of Policy Evaluation (JPEVAL)*](https://polipapers.upv.es/index.php/jpeval/). It is a biannual scientific journal dedicated to the evaluation and monitoring of public policies through the publication of theoretical and empirical research articles, as well as reviews and sections on best practices related to policy evaluation and monitoring.

My research interests are economics of education, labour economics, gender economics, applied microeconometrics, data science and (impact) policy evaluation.

<div class="callout callout-success" markdown="0">
<div class="callout-title">{% include icon.html name="award" class="callout-icon" %} Best communication award, 2025</div>
<p>Award for the best communication on the Sustainability of Public Policies axis at the 4th Congress of Economy and Business of Catalonia (Col·legi d'Economistes de Catalunya).</p>
</div>

{% capture selected %}{% bibliography --query @*[selected=true] %}{% endcapture %}
{% if selected contains "pub-entry" %}
## Selected research

<div class="section-card selected-pubs" markdown="0">
{{ selected }}
<p style="margin: var(--space-4) 0 0;"><a href="{{ '/research/' | relative_url }}">All research &rarr;</a></p>
</div>
{% endif %}
