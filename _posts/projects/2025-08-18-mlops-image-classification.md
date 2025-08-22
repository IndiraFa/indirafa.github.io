---
layout: page-fullwidth
header:
    image_fullwidth: mlops_header_unsplash.jpg
    title: "Image classification continuous training pipeline"
subheadline: "MlOps"
teaser: This project was realized for the MLOps course of the specialized Master’s AI, Data expert & MLops at Telecom Paris. The objective is to develop a complete machine learning pipeline for image classification. It handles a binary classification task on an image dataset containing labeled “dandelion” and “grass” images. A development environment was set for development and tests and a production environment allows the deployment on a Kubernetes cluster.
tags:
    - MlOps
    - Airflow
categories:
    - projects
---


## 1. Global project architecture

![Global scheme](/images/mlops/global_scheme.png)

Airflow orchestrates the pipeline for the download and storage of the database, training and serving of the model and retraining when new images are available. Monotoring of training is available through MlFlow. The classifier can be accessed via an API and via a Streamlit interface. New images uploaded to Streamlit are stored for further retraining of the model. Prometheus along with Grafana are used for the monitoring of the API use and Streamlit interactions. Deployment is set up on a Kubernetes cluster using Helm Charts (see part 10.)


The full code and step by step setup is available on [<img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="20" style="vertical-align:middle"> GitHub](https://github.com/nnmp020395/MLOps_ClassifiImage)


## 2. Dataset

The training images are collected from public datasets and user uploads. All data is centralized in an S3-compatible MinIO bucket, ensuring scalability, high availability, and seamless integration with the MLOps pipeline.

The core dataset used in this project consists of RGB images labeled as either "dandelion" or "grass", intended for binary image classification, available on [github](https://github.com/btphan95/greenr-airflow/tree/master/data).

![Image1](/images/mlops/grass_example.jpg)
![Image2](/images/mlops/dandelion_example.jpg)
*Example of grass and dandelion images from the database*

Starting from this database, the training and validation sets are automatically curated and stored in MinIO.

The training database is also dynamically enriched using images added through the Streamlit interface. Labels of images are manually validated by an admin before being used for retraining.


## 4. Storage

### - PostgreSQL
A PostgreSQL database keeps track of all the processed images. The **plants_data** table includes the following fields:

| Column Name | Description                       |
| ----------- | --------------------------------- |
| id          | Unique identifier (auto)          |
| url\_source | Original name or source URL       |
| label       | Image class (**dandelion**/**grass**) |
| url\_s3     | Full MinIO path to the image      |



### - Minio Bucket: 

MinIO is used as the central object store for both training/inference images and serialized model weights. It provides a lightweight, self-hosted, and S3-compatible storage solution integrated into the entire MLOps workflow.

The project stores data inside the image-dandelion-grass bucket, following the structure below:

``` bash
image-dandelion-grass/
├── model/
│   ├── YYYY-MM-DD/               # Folder per training date
│   │   ├── dinov2_classifier_0.pth
│   │   ├── dinov2_classifier_1.pth
│   │   └── ...                   # Multiple versions for each training session
├── raw/
│   ├── dandelion/                # Manually validated images labeled "dandelion"
│   ├── grass/                    # Manually validated images labeled "grass"
│   └── new_data/
│       ├── pending_validation/   # User-submitted images awaiting admin validation
│       └── corrected_data/       # Admin-labeled and approved images
```

A DAG in Airflow periodically checks for new validated images in **corrected_data/**. When 10 or more new images are available, they are:

1. Moved to their appropriate class folders (**raw/dandelion/** or **raw/grass/**),
2. Registered in the PostgreSQL database, which stores metadata including the file name, label, and full MinIO URL,
3. Used to automatically retrain the classification model (see figure below)

![Retrain model](/images/mlops/retrain_model_schema.png)


## 5. Model training

Our goal is to build a binary image classification model that distinguishes between images of **dandelion** and **grass**. \
The input to the model is a single RGB image, resize to 224x224 pixels and the output is a binary prediction : either class 0 (dandelion) or class 1 (grass).

### Architecture overview
We use the DINOv2 vision transformer model as a feature extractor. DINOv2 is a self-supervised vision transformer retrained on large-scale image datasets. Specifically, we use the ViT-S/14 variant of DINOv2 without fine-tuning its internal weights. Instead, we extract a feature embedding from the [CLS] token of the last transformer layer. The DINOv2 output is passed through a simple classification head. We freeze the DINOv2 backbone and train only the classification head. The model is trained using binary cross-entropy loss, optimized with Adam at learning rate of 0.003.


The model achieves more than **90%** accuracy on the test set and shows good generalization on both sunny and shaded outdoor scenes.


### Training monitoring with Mlflow

Mlflow is used for the monitoring of the training.

The page is designed to allow monotoring of different training experiments, and displays the evolution of train accuracy and train loss for each experiment, along with the validation accuracy value.

![Mlflow page](/images/mlops/main_mlflow.png)


<div style="display: flex; justify-content: space-between;">
  <img src="/images/mlops/train_loss.jpeg" alt="Train loss" width="49%" />
  <img src="/images/mlops/val_acc.jpeg" alt="Validation accuracy" width="49%" />
</div>


Acess to a specific run gives detailed information regarding the date of the run, duration, status, training parameters and metrics, as depicted below.

<div style="display: flex; justify-content: center;">
  <img src="/images/mlops/mlflow_kpi.png" alt="Mlflow kpi" width="800" />
</div>


## 6. Automated pipeline with Airflow

- Dev environment :

Airflow webserver is accessible at the url: http://localhost:8080

![All dags](/images/mlops/all_dags.png)

Three dependent dags are set to download the database, train the model, and trigger training if there are more than 10 new images in the database.

<div style="display: flex; justify-content: center;">
  <img src="/images/mlops/dag_dependencies.png" alt="Dag dependencies" width="600" />
</div>


The first dag is useful at the beginning to download the images to Minio and create the SQL database to store the urls and labels of the images.


![Dag1](/images/mlops/dag1.png)

The last task triggers the training of the model with the downloaded images (second dag below).

![Dag1](/images/mlops/dag_train.png)


Once per week, the third dag (below) is triggered and checks if there are more than 10 new images in the database. If there are less than 10, it skips the 2 last tasks, other wise it moves the new images in the training folder and triggers the training dag.

![Dag2](/images/mlops/dag2.png)


## 7. Inference on the model

There are two ways for the user to interact with the model in inference mode : via te API or via the streamlit interface. 


- Dev environment: 

The API is accessible at the url : http://localhost:8000. A prediction can be made using the following command :

```bash
curl -X POST http://localhost:8000/predict \
  -H "accept: application/json" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@/path/to/your/image.jpg"
```


The Streamlit app is accessible at the url : http://localhost:8501. A prediction can be obtained by directly drag and dropping an image or uploading from your local machine.


![Streamlit page 1](/images/mlops/streamlit_page1.png)

A second page, accessible with a password, allows an admin to check the label attributed to submitted images, correct it if necessary, and store the correctly labeled image in the minio database. These new images are then use for retraining the model.

![Streamlit page 2](/images/mlops/streamlit_page2.png)


## 8. Monitoring of usage

Monitoring of the API usage is setup using Prometeus and Grafana. Prometheus scraps the metrics of API usage and Streamlit interactions while Grafana connects to Prometheus as a data source and is used to build dashboards.


In the development environment the dashboards are accessible at the url : http://localhost:3000


### API monitoring

The API monitoring dashboard is set up to display POST and GET requests (predictions fall under the POST requests category). It combines numbers for the requests done directly to the API and requests done through Streamlit.

![API monitoring](/images/mlops/api_monitoring.png)

### Streamlit monitoring

The Streamlit monitoring dashboard is set up to display page views and total predict button clicks.

![Streamlit monitoring](/images/mlops/streamlit_monitoring.png)


## 9. Production environment

The production deployment involves deploying these services to a local Kubernetes cluster via Helm Chart. For stable, reusable, and versioned production, Helm is preferred over the `kubectl apply` approach. YAML files are developed using official charts compatible with different applications.

We chose the Docker-desktop cluster, which is provided by Docker Desktop and allows us to deploy directly without the need to define a virtual machine or other installations.

Te step by step deployment is explained on [<img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" width="20" style="vertical-align:middle"> GitHub](https://github.com/nnmp020395/MLOps_ClassifiImage)

## 10. Conclusion and next steps

This project allows to set up a functioning MlOps pipeline for a classification model.

To further improve it, we could in the future integrate a vector database to store and manage image embeddings within a feature store. By extracting feature vectors from images using our trained model and storing them in a vector database such as Chroma, FAISS, Pinecone, or Weaviate, we could enable mode efficient similarity search, and deduplication when storing images uploaded on streamlit. Incorporating this into the MLOps workflow would make it more scalable and production-ready. 

Another improvement would be to save the retrained model on MinIO and expose the it on the API only if certain criteria are met (for example, a threshold on test accuracy). This safeguard has not yet been implemented. 

As food for thought, whereas the dandelion/grass classification task is quite easy, we could also try harder tasks such as rocket salad / dandelion leaves classification, what do you think : 

![Roquette](/images/mlops/roquette.jpeg)
![Pissenlit](/images/mlops/pissenlit.jpeg)
*Dandelion or Rocket Salad ???*

