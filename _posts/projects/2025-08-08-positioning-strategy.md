---
layout: page-fullwidth
header:
    image_fullwidth: food_header_unsplash.jpg
    title: "Positioning strategy"
subheadline: "Data exploration, analysis and recomendations"
teaser: Complete Streamlit Webapp to showcase strategic analysis of the database of a leader in B2C organic recipe recommendations

# image:
#     thumb:  homepage_typography-thumb.jpg
#     homepage: homepage_typography.jpg
#     caption: Image by Antonio
#     caption_url: "http://www.aisleone.net/"
breadcrumb: true
tags:
    - streamlit
    - webapp
    - preprocessing
    - visualization
categories:
    - projects
---
<!--more-->

<div class="row">
<div class="medium-5 medium-push-7 columns" markdown="1">
<div class="panel radius" markdown="1">
**Table of Contents**
{: #toc }
*  TOC
{:toc}
</div>
</div><!-- /.medium-4.columns -->

<div class="medium-7 medium-pull-5 columns" markdown="1">

## Project Context
Mangetamain, a leader in B2C organic recipe recommendations, released part of its database to the public. Our mission was to build an interactive web application that delivers advanced insights from this dataset while showcasing strong technical expertise. Developed collaboratively with data scientists, the project emphasizes best coding practices, teamwork on Git, and a high standard of technical excellence.

Our team chose to explore the nutritional values of the recipes and identify strategic development axis for the company. 

The dataset used is available on [kaggle](https://www.kaggle.com/datasets/shuyangli94/food-com-recipes-and-user-interactions).

</div>


## Setup

The complete project code and setup process is available on [<img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="20" style="vertical-align:middle"> GitHub](https://github.com/IndiraFa/PositioningStrategy_webapp) (please note that you will need to host the database).
This results in an interactive web app that enables data exploration and guides the user through the main conclusions of our study. 

[🚀 Interactive live webapp >](https://positioningstrategy-mangetamain-stable.streamlit.app)

Below is a step-by-step description of the analytical process, along with visuals of the app. 

## Homepage
<br>
> The Homepage shows the distribution of the Nutriscore values in the dataset, both before and after removing outliers. The normality of the distribution can be evaluated using three different tests (Shapiro-Wilk, Kolmogorov-Smirnov, Anderson-Darling). Based on the Nutriscore values, labels are assigned for easier readability, and pie charts display their distribution. This analysis shhows that Mangetamain already has the potential to build a strong brand around healthy food.
Around 90% of the recipes have a Nutriscore of A, B or C, which is a strong indicator of the nutritional quality of the recipes featured on the website. 


![Homepage1](/images/kitbigdata/homepage1.png)
![Homepage2](/images/kitbigdata/homepage2.png)
![Homepage3](/images/kitbigdata/homepage3.png)


## Outliers detection and preprocessing

<br>
> Data cleaning to remove outliers based on nutritional information is carried out in two steps. First, manual filtering is applied using thresholds beyond which outliers are identified.
Then, the Z-score method is used to detect additional outliers. In total, this process removes about  10% of the recipes.

![Outlier1](/images/kitbigdata/outlier1.png)
![Outlier2](/images/kitbigdata/outlier2.png)
![Outlier3](/images/kitbigdata/outlier3.png)
![Outlier4](/images/kitbigdata/outlier4.png)

## Nutritional data quality assessment

<br>
> Nutritional data quality is essential for calculating the Nutri-Score.
However, the values provided by users may be inaccurate. To assess the quality of tihs data, we conducted a linear regression analysis. The model was trained using nutritional information from the recipes (such as protein content, total fat, and carbohydrates) alongside the calories per serving, since a known linear relationship exists between these variables.[^1] We used the dataset without outliers to incorporate the initial preprocessing steps. 
We then performed confidence interval tests, with a configurable confidence level, using the bootstrap method on the obtained coefficients to determine whether they differ significantly from the reference values. The app allows the user to chose a custom confidence interval. The results show that the coefficients are indeed significantly different from the reference values.



[^1]:Amount of calories per gram of fat, carbohydrate, and protein <https://www.nal.usda.gov/programs/fnic>

![Quality1](/images/kitbigdata/quality1.png)
![Quality2](/images/kitbigdata/quality2.png)
![Quality3](/images/kitbigdata/quality3.png)
![Quality4](/images/kitbigdata/quality4.png)
![Quality5](/images/kitbigdata/quality5.png)

## Correlation analysis

<br>
> We then explored the linear correlation between the calculated nutritional score and the characteristics of the recipesa and the correlation between the calculated nutritional score and the interactions of the users (reviews and scores). This analysis shows some correlations between the nutritional data that are expected and to the chemical composition of each nutrient.
The correlations between the Nutriscore and the nutritional data are in line with the way the Nutriscore was calculated (as a linear combination of nutritional variables).
No correlation is seen with the number of ingredients, of steps or the preparation time.
Finally, no strong correlation were observed with the interactions of the users of the website. Nevertheless, the lowest labels have the best interaction per recipe ratio, and we can conclude that all kinds of recipes are enjoyed, no matter their Nutriscore.
We recommend Mangetamain to keep a variety of recipes on the website, even if they want to promote healthy eating, beacause lower nutriscore recipes may drive trafic and interaction. 


![Correlation1](/images/kitbigdata/correlation1.png)
![Correlation2](/images/kitbigdata/correlation2.png)
![Correlation3](/images/kitbigdata/correlation3.png)
![Correlation4](/images/kitbigdata/correlation4.png)
![Correlation5](/images/kitbigdata/correlation5.png)

## Tag analysis

<br>
> Filled in by users, the labels describe the nature, type or main cooking method of the recipes, e.g 'vegetarian', 'dinner', 'course' (see the word cloud below).
We explored the information from these labels that is considered useful for health. We have associated each recipe with a rating and a label, the app will automatically sort the recipes with the selected labels, analyze the distribution of nutriscores, the highest nutriscores and then focus on the distribution of nutrients according to the label.
We conclude from this analysis that the labels are clearly consistent with our nutriscore calculated previously. For instance, "Low protein" recipes cannot offer a good ratio with nutrients and are therefore rated C or D. But, with a "low cholesterol" recipe, the rating is now A.

![Tag1](/images/kitbigdata/tag1.png)
![Tag2](/images/kitbigdata/tag2.png)
![Tag3](/images/kitbigdata/tag3.png)
![Tag4](/images/kitbigdata/tag4.png)


## Appendix

<br>
> The Appendix page details the method used in our study to calculate the Nutriscore, along with examples.

![Appendix1](/images/kitbigdata/appendix1.png)
![Appendix2](/images/kitbigdata/appendix2.png)
![Appendix3](/images/kitbigdata/appendix3.png)


