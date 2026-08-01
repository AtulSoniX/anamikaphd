---
layout: page
permalink: /publications/
title: publications
description: This page shows my publications (journals, conference proceedings, and softwares).
nav: true
nav_order: 3
---

<!-- _pages/publications.md -->

{% include bib_search.liquid %}

<div class="publications">

<h2>Journal Articles</h2>

{% bibliography --query @article %}

<h2>Conference Proceedings</h2>

{% bibliography --query @inproceedings %}

<h2>Software</h2>

{% bibliography --query @misc %}

</div>
