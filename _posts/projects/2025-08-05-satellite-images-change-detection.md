---
layout: page
header:
   image_fullwidth  : "nasa_header_unsplash.jpg"
   title: Satellite imagery
sidebar: left
sidebar_file: _sidebar_custom.html
subheadline: AI & ML
title:  "Unsupervised change detection in satellite images time series"
teaser: "Project in collaboration wit Airbus Defence and Space during Specialized Msc  Artificial Intelligence - MlOps and Data Expert at Telecom Paris. Many thanks to Airbus and Telecom Supervisors and to the whole contributing student team (Phuong N., Martin L. C., Ghalia R. C.)."
breadcrumb: true
tags:
    - remote sensing
categories:
    - projects
image:
    thumb: gallery-example-3-thumb.jpg
    title: gallery-example-3.jpg
    caption_url: http://unsplash.com
---

## 1. Context and objectives 
For teams dedicated to analyzing satellite imagery and detecting changes, the large temporal and spatial scale makes it tedious to identify the most relevant changes to analyze (e.g., the appearance of a deforestation zone, detection of a new building/road in an area of interest, flooding/drying of land).
The proposed approach is as follows:

- Extract local image representations using large visual or multimodal foundation models
- Detect changes in local representations from one image to another within a time series
- Cluster the variations in local representations
- Detect the most relevant atypical variations by ranking them based on rarity or magnitude of change


The analyst can then select a change from among the atypical variations detected by the model and focus their attention on this type of change at other points in the time series or in another area (another time series).

After exploring the Dynamic Earth Net dataset, the study compared the performance of various foundation models in detecting labeled changes in the ground truth. The most effective models were then used to extract time series of satellite image embeddings to implement unsupervised change detection methods.

<!--more-->

~~~
show_meta: true
~~~

If you don't want to show metadata, it's simple again:

~~~
show_meta: false
~~~


## 2. Data pre processing

xx

## 3. Fondation models benchmark

xx

## 4. Quantitative analysis
xx


## 5. Automatic change detection methods

xx
