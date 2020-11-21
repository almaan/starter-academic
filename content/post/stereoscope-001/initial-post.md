---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Publication | Spatial Single Cell Mapping"
subtitle: ""
summary: ""
authors: [admin]
tags: []
categories: []
date: 2020-10-09T11:05:04+01:00
lastmod: 2020-10-09T11:05:04+01:00
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

Delighted to say that our manuscript "_Single-cell and spatial transcriptomics
enables probabilistic inference of cell type topography_" is now published in
_Communications Biology_ and thus out there for everyone to read in its final
form. This is the paper that describes the theoretical underpinnings for
[stereoscope](https://github.com/almaan/stereoscope), which allows you to
spatial map cell types found in single cell data onto spatial transcriptomics
data. There is also a nice bit of discussion as to whether spatial
transcriptomics data (ST/Visium) can be properly modeled using a negative
binomial distribution (which we use) found in the supplementary (spoiler, it
seems like it). Have a look at it if you are interested!


 - Manuscript Link : [HERE](https://www.nature.com/articles/s42003-020-01247-y)
 - stereoscope Link : [HERE](https://github.com/almaan/stereoscope)

P.S: I'm also planning to update the code base in the future, providing an API for
`scanpy`, but also add some features like subsampling and gene selection to the
standard modules; but that is for later.
