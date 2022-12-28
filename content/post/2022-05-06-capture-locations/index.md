---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "What is thy name?"
subtitle: "A call for unison"
summary: "A brief discussion about the naming convention of the observations in spatial transcriptomics." 
authors: ["admin"]
tags: ["reflections"]
categories: ["reflections"]
date: 2022-05-06T06:01:05+01:00
lastmod: 2022-05-06T06:01:05+01:00
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
projects: ["reflections"]
---

You know that feeling when you're frustrated over something that has absolutely
no real importance, but you just can't let it go. That's me right now. <span
style="color:#86fc8e">I'm seriously bothered by the fact that every single
publication seems to use a different term to denote the locations in
spatial transcriptomics data.</span>

It's not that people aren't using _my favorite_ term (FYI, I don't have one)
that annoys me, but the fact that there's no consensus on which term to use.
There are _beads, cells, features, voxels, pixels, spots, capture locations_,
and many other innovative alternatives. The current practice is **confusing**,
and settling on one term should not be that hard.

> "If names are not correct, language will not be in accordance with the truth of things."
> -- Confucius

My idea with this post was to present some of the different terms I've
encountered so far (disclaimer: this is not an exhaustive list**, together with a
bit of personal commentary - mainly to provide some context. To then couple this
with a poll of some sort, and see what the thoughts of the community are.
Perhaps this could spark some sort of discussion or reflection on the topic.

**NOTE** : this list was updated (2022-05-09) after posting
[this](https://twitter.com/aalmaander/status/1523290638422188034?s=20&t=04LQK9AF-yQHLcgYT_K2dA)
twitter post, where some other suggestions were presented. Thanks to all the
people who contributed to a really nice discussion!


* **Bead** : popularized by
  [Slide-seq](https://www.science.org/doi/10.1126/science.aaw1219), in which
  beads are used to capture the transcripts within the tissue. This also
  resonates well with data from the HDST platform. However, it makes zero sense
  with data from other platforms, like Visium, where capture probes are
  immobilized to a solid surface, nor to imaging-based techniques like MERFISH or
  SeqFISH where single molecules are visualized and there's no "capture" going
  on.

* **Spot** : is used somewhat ambiguously in different techniques. In the
  [ST](https://www.science.org/doi/10.1126/science.aaf2403) and
  [Visium](https://www.10xgenomics.com/products/spatial-gene-expression)
  platform, the mRNA capture locations are printed onto the surface in a fashion
  similar to how
  [microarrays](https://www.nature.com/scitable/definition/microarray-202/) were
  constructed, which made the use of "spot" to describe the capture locations
  quite natural. But just as "beads" makes little sense to describe the capture
  locations in Visium data, "spots" makes no sense in Slide-seq data. What adds
  another layer of confusion is that in [imaging-based
  methods](https://www.nature.com/articles/s41598-019-43943-8), "spot" is
  sometimes used to describe the signal from a single molecule. This kind of
  ambiguity adds even more confusion to the already confusing situation. "Spot"
  is currently (2022-05-06) favored by 10x Genomics (the distributor of Visium).
  10x Genomics has a large reach and, i.e., whatever term they chose, a lot of
  people will use it.
  
* **Feature** : have been used by some authors to describe the spatial
  locations, for example, [this](https://doi.org/10.1016/j.molcel.2021.03.016)
  review. Without bashing too hard on the people who prefer this term, I must
  confess that I find it extremely impractical to use the word "feature" to
  describe the "observations". To me, a feature is the information associated
  with the spatial location (i.e., gene expression). Also, most analysis
  frameworks (e.g., Seurat) tend to call the genes (if you're analyzing gene
  expression data) features. If we were to use the term feature also for our
  spatial locations, would we then say that we have a "feature times feature"
  matrix? That's not very helpful. If confusion is something we want to avoid,
  we should not pursue the use of this term.

* **Pixel** : a term that most of us are familiar with, but usually in the
  context of pictures or camera lenses. The word pixel is a portmanteau of the
  two words "picture" and "element" (where "pix" is used as short form of
  "picture"). I first saw this being used in the [RCTD
  paper](https://www.nature.com/articles/s41587-021-00830-w), but have also
  heard other people use the term in presentations. To some extent, this is a
  slightly more general term than "bead" or "spot", and it works with
  imaging-based spatial transcriptomics methods. Also, to some extent, you could
  consider the Visium or Slide-seq data as an "image" with ~20,000 channels (one
  channel - one gene) instead of the standard three channels (RGB), but that's a
  bit of a stretch. Calling the spatial data a "picture" when it's not really
  image data that we're working does not really vibe with me.

* **Voxel** : is a lesser known relative to the pixel, where "vo" for *volume*
  has replaced "pix" for picture; giving us a _"volume element"_. My first
  exposure to the usage of this term to denote spatial locations was the
  [Tangram paper](https://www.nature.com/articles/s41592-021-01264-7). I'm not
  fully sure why this term was preferred over pixel, but perhaps "volume" was
  considered a more general term that could describe elements in both 2D and 3D
  methods (if you consider area as a "2D volume"). Although, I believe, for most
  people, volume relates very much to 3D objects; thus, potentially puzzling
  some people.
  
* **Spatial location** : sometimes, often in an attempt to be method agnostic, I
  (and others) have used this term instead of any of the aforementioned
  alternatives. It definitely does the trick, it states exactly what we're
  dealing with and there's no room for confusion. But it's terribly ~~unsexy~~
  boring, as well as being quite long (two words!) and inconvenient to use.
  Sure, one could go for yet another portmanteau, but referring to spatial locations
  as _"splocs"_ sounds... ehm... ridiculous. 

* **Gexel** : a what? Yeah you read that correct, a _gexel_. I stumbled upon
  this term when reading the 2021 [MULTILAYER
  paper](https://www.sciencedirect.com/science/article/pii/S2405471221001514),
  where the authors got inspired by the pixels and conceived (I could not find
  any earlier use) of the "gexel", being short for **"gene expression
  element"**. At first I kind of frowned at this term, but the more I've been
  thinking about it, the more I've come to like it. It's fully method agnostic,
  it's short and convenient to use, and "gene expression element" captures the
  essence of what our spatial locations are. The big downside to this term is
  its lack of _generalization to other modalities_, or multimodal assays where
  different kinds of information are registered at the same location. Perhaps
  one could use _daxel_ for "data element" or _spexel_ for "spatial expression
  element", but both sound a bit "forced".
  
We can summarize this is a very basic table:
  
| Term             | Agnostic | Descriptive | Unambiguous | Convenient | Multimodal |
| ---              | ---      | ---         | ---         | ---        | ---        |
| bead             | no       | no          | yes         | yes        | yes        |
| spot             | no       | no          | no          | yes        | yes        |
| feature          | yes      | no          | no          | yes        | yes        |
| pixel            | yes      | no          | yes         | yes        | yes        |
| voxel            | yes      | no          | yes         | yes        | yes        |
| spatial location | yes      | yes         | yes         | no         | yes        |
| gexel            | yes      | yes (+)     | yes         | yes        | no         |


Columns: Agnostic - method agnostic, Descriptive - conveys information about what it
represents, Unambiguous - will not be confused with other elements of the data,
Convenient - easy to use and say, Multimodal - can be used in mulitmodal
assays.


P.S. I'm acutely aware of the fact that _"being bothered by the inconsistency in
naming of spatial locations"_ would be the perfect answer to _"tell me you have
no real problems without saying you have no real problems."_ Other _more
important_ things are happening around the world, I acknowledge that 100%, but I
don't want those things to engulf my whole existence. Silly discussions must
also be allowed, just as happiness must be allowed space in times of grief and
sorrow.

