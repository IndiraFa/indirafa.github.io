---
layout: page-fullwidth
header:
   image_fullwidth  : "bioscience_header_unsplash.jpg"
   title: Explainability in medical imaging
subheadline: AI & ML
title:  "Explainability - Visualisation of concepts activated in the classification of medical images"
teaser: "This project was conducted as part of the Specialized Master in Artificial Intelligence, Data Expert and MLOps at Telecom Paris."
tags:
    - medical imaging
    - explainability
    - histopathology
categories:
    - projects
---

The full code and step by step setup is available on [<img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="20" style="vertical-align:middle"> GitHub](https://github.com/IndiraFa/XAI_medical_images_viscoin)

 
 
## Project summary

This work is the implementation of the VisCoIN model [^1], a method that relies on a generative model (StyleGAN2) and a classifier (ResNet50), developed for visualizing the concepts activated during image classification. The first objective is to assess the reproducibility of the published results on the CUB-200 dataset. The second objective is to evaluate the transferability of the VisCoIN model to medical images, specifically using the NCT-CRC-HE dataset, and to assess its relevance as a tool for understanding the decision-making process of deep learning models in the context of medical image analysis. We were able to reproduce the published results to a certain extent. However, the obtained FID score was significantly worse than the one reported in the original paper, leading to a struggling reconstruction, and we provide possible explanations for this discrepancy. Transferring the VisCoIN model to medical images proved to be more difficult. While we obtained some results indicating that the methodology is relevant, the overall architecture likely needs to be adaped to better suit the characteristics of medical images in order to produce more usable outcomes.




[Read the full report here](https://github.com/IndiraFa/XAI_medical_images_viscoin/blob/main/VisCoIN_implementation_medical_images.pdf)



[^1]:  <https://arxiv.org/html/2407.01331v1>