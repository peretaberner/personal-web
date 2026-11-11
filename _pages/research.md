---
title: "Research"
layout: page
permalink: /research/
---

# Research

<input type="text" class="pub-search" id="pubSearch" placeholder="Filter by title, author, or year...">

<div class="section-card" id="pubList">
<h2>Peer-Reviewed Published Papers</h2>

{% bibliography --query @article %}

<h2>Working Papers</h2>

{% bibliography --query @techreport %}

<h2>Book Chapters</h2>

{% bibliography --query @incollection %}

<h2>Conference Papers</h2>

{% bibliography --query @inproceedings %}

<h2>Work in Progress</h2>

{% bibliography --query @unpublished %}
</div>
