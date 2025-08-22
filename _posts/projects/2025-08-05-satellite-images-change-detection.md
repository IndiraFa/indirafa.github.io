---
layout: page-fullwidth
header:
   image_fullwidth  : "nasa_header_unsplash.jpg"
   title: Satellite imagery
subheadline: AI & ML
title:  "Unsupervised change detection in satellite images time series"
teaser: "Project in collaboration wit Airbus Defence and Space during Specialized Msc  Artificial Intelligence - MlOps and Data Expert at Telecom Paris. Many thanks to Airbus and Telecom Supervisors and to the whole contributing student team. "
tags:
    - remote sensing
    - change detection
categories:
    - projects
---

<!-- The full code and step by step setup is available on [<img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="20" style="vertical-align:middle"> GitHub](https://github.com/IndiraFa/change_detection_sat_images) -->

## 1. Context and objectives 
For teams dedicated to analyzing satellite imagery and detecting changes, the large temporal and spatial scale makes it tedious to identify the most relevant changes to analyze (e.g., the appearance of a deforestation zone, detection of a new building/road in an area of interest, flooding/drying of land).
The proposed approach is as follows:

- Extract local image representations using large visual or multimodal foundation models, bot general and in-domain (SAM2, DinoV2, Clay, Anysat)
- Detect changes in local representations from one image to another within a time series
- Cluster the variations in local representations
- Detect the most relevant atypical variations by ranking them based on rarity or magnitude of change

The analyst can then select a change from among the atypical variations detected by the model and focus their attention on this type of change at other points in the time series or in another area (another time series).

After exploring the Dynamic Earth Net dataset, the study compared the performance of various foundation models in detecting labeled changes in the ground truth. The most effective models were then used to extract time series of satellite image embeddings to implement unsupervised change detection methods.

<!--more-->

## 2. Dataset and preprocessing

We used for this project the Dynamic Earth Net Dataset (see Figure 1)[^1]. It can be downloaded [here](https://mediatum.ub.tum.de/1650201).

![Dataset](/images/satellite/dataset.png)
*Figure 1 - Visualization of the Dynamic Earth Net Dataset*

 This dataset consists of daily, multi-spectral satellite observations of 75 selected areas of interest distributed over the globe with imagery from Planet Labs. These observations are paired with pixel-wise monthly semantic segmentation labels of seven land use and land cover (LULC) classes.


After carefully checking the alignment of the images, necessary for our change detection task, we estimated ground truth changes based on the LULC labels.

[^1]: Toker, Aysim, et al. "Dynamicearthnet: Daily multi-spectral satellite dataset for semantic change segmentation." Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition. 2022, <https://arxiv.org/pdf/2203.12560>



## 3. Fondation models benchmark

Image embeddings were generated using four foundation models : 
- SAM2 (general)
- DinoV2 (general)
- Clay (trained on satellite images)
- Anysat (trained on satellite images)

We compared the evolution of embeddings per patch to aggregated ground truth changes per patch to evaluate each model’s performance.

![Comparison Method](/images/satellite/comparison.png)
*Figure 2 - Comparison Methodology between extracted embeddings and ground truth*

Results were analyzed across different types of changes, including various temporal and semantic patterns. A detailed case study is presented in the next section.

## 4. Use case

The selected use case demonstrates an extended drought phenomenon, which is clearly visible in the satellite images (see Figure 3) and also reflected in the ground truth (see Figure 4).

![Use Case](/images/satellite/usecase_images.png)
*Figure 3 - Extract of satellite images for the usecase*


![Use Case](/images/satellite/usecase.png)

*Figure 4 - Ground truth evolution over time for the case study*

The ROC curve for this case study shows better performance for the two in-domain foundation models: Clay and Anysat (see Figure 5).

<img src="/images/satellite/ROC.png" alt="ROC Curve" width="500"/>

*Figure 5 - ROC Curve obtained for the case study*

Patch-level analysis shows a strong tendency for SAM2 and DinoV2 to produce false positives. Their larger patch size also makes detection less precise (see Figure 6).

![Models performance](/images/satellite/model_perf.png)
*Figure 6 - Comparison of the performance of the four models on the drought usecase*


## 5. Automatic change detection

We tested a local change detection method per patch by comparing new observations to reference observations, as illustrated in Figure 7.

![Local change method](/images/satellite/local_method.png)
*Figure 7 - Local change detection method*

Figure 8 shows that two clusters are clearly identified: before and after the change. Both the distance-based method and the LOF (Local Outlier Factor) score allow for change identification, with the LOF method better isolating noise from real changes.

![Local change results](/images/satellite/local_results.png)
*Figure 8 - Results of the local change detection method on the case study*

## 6. Conclusion

Our study demonstrates that foundation models trained specifically on satellite imagery, such as Clay and Anysat, outperform general-purpose models in detecting changes over time. 

By extracting local patch-based embeddings, we were able to generate time series that facilitate unsupervised change detection, with the LOF-based method proving quite effective at distinguishing real changes from noise. 

These results highlight the potential of combining in-domain foundation models with unsupervised techniques to efficiently identify relevant changes in large-scale satellite datasets.

Future work should focus on testing the robustness of these methods on reconstructed images or images with varying lighting conditions, refining the distinction between semantic and non-semantic changes, and further evaluating the performance of Clay and Anysat across different types of changes. 

Additionally, we aim to develop a hierarchical approach to change detection, clustering different types of changes and prioritizing them based on their atypicality.