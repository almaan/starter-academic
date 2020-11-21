---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Manuscript | Spatial Exploration of Her2 Breast Cancer"
subtitle: ""
summary: ""
authors: [admin]
tags: []
categories: []
date: 2020-07-15T11:05:04+01:00
lastmod: 2020-07-15T15:05:04+01:00
featured: false 
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

We've put out a new
[manuscript]("https://www.biorxiv.org/content/10.1101/2020.07.14.200600v1.article-metrics") on bioRxiv, where the expression landscape of HER2-positive
breast cancer tumors is studied from a spatial perspective. This study contains
several examples of how spatial data may be analyzed and what information beyond
the obvious - like the spatial distribution of a certain gene - that can be
mined from it. My hope is that anyone looking for ideas regarding how to analyze
their own spatial data find this useful and relevant, irregardles of their
interest in breast cancer. In the study we cluster spots by gene expression, but
also take this one step further, using the clusters to find <i>core
signatures</i> of immune and tumor cell populations within our samples. We also
use <i>stereoscope</i> to map single cell data down onto our tissue sections,
information which then is used to indentify potential TLS-sites as well as to
study patterns of cell type co-localization. All the scripts and a neat app for
visualization of the results can be found [here]("https://github.com/almaan/her2st/").

