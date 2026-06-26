# Amazon SageMaker <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. End-to-End ML Service](#2-end-to-end-ml-service)
- [3. Built-in Algorithms (extract)](#3-built-in-algorithms-extract)
- [4. Automatic Model Tuning (AMT)](#4-automatic-model-tuning-amt)
- [5. Model Deployment \& Inference](#5-model-deployment--inference)
- [6. Model Deployment Comparison](#6-model-deployment-comparison)
- [7. SageMaker Studio](#7-sagemaker-studio)
- [8. Data Wrangler](#8-data-wrangler)
- [9. What are ML Features?](#9-what-are-ml-features)
  - [9.1. Feature Store](#91-feature-store)
- [10. Clarify](#10-clarify)
  - [10.1. Model Explainability](#101-model-explainability)
  - [10.2. Detect Bias (human)](#102-detect-bias-human)
  - [10.3. Different kind of biases (definitions)](#103-different-kind-of-biases-definitions)
- [11. Ground Truth](#11-ground-truth)
- [12. ML Governance](#12-ml-governance)
  - [12.1. Model Dashboard](#121-model-dashboard)
  - [12.2. Model Monitor](#122-model-monitor)
  - [12.3. Model Registry](#123-model-registry)
- [13. Pipelines](#13-pipelines)
- [14. JumpStart](#14-jumpstart)
  - [14.1. ML Hub](#141-ml-hub)
  - [14.2. ML Solutions](#142-ml-solutions)
- [15. Canvas](#15-canvas)
  - [15.1. Ready-to-use models](#151-ready-to-use-models)
- [16. MLFlow on Amazon SageMaker](#16-mlflow-on-amazon-sagemaker)
- [17. Summary](#17-summary)
- [18. Extra Features](#18-extra-features)
- [19. Savings Plan](#19-savings-plan)

# 1. Introduction

- Fully managed service for developers / data scientists to build ML models.
- Typically, difficult to do all the processes in one place + provision servers.
- **Machine learning process (simplified):** Predicting our exam score.
  - [Amazon SageMaker](/Images/Machine%20Learning/AmazonSageMaker.png)

# 2. End-to-End ML Service

- Collect and prepare data.
- Build and train machine learning models.
- Deploy the models and monitor the performance of the predictions.
  - [Amazon SageMaker - End-to-End ML Service](/Images/Machine%20Learning/AmazonSageMakerEndToEndMlService.png)

# 3. Built-in Algorithms (extract)

- **Supervised Algorithms**
  - Linear regressions and classifications.
  - KNN Algorithms (for classification).
- **Unsupervised Algorithms**
  - **Principal Component Analysis (PCA):** Reduce number of features.
  - **K-means:** Find grouping within data.
  - **Anomaly Detection.**
- **Textual Algorithms:** NLP, summarization...
- **Image Processing:** classification, detection...

# 4. Automatic Model Tuning (AMT)

- Define the **Objective Metric**.
- AMT automatically chooses hyperparameter ranges, search strategy, maximum runtime of a tuning job, and early stop condition.
- Saves us time and money.
- Helps us not wasting money on suboptimal configurations.

# 5. Model Deployment & Inference

- Deploy with one click, automatic scaling, no servers to manage (as opposed to self-hosted).
- **Managed solution:** Reduced overhead.
- **Real-time**
  - One prediction at a time.
- **Serverless**
  - Idle period between traffic spikes.
  - Can tolerate more latency (cold starts).
- **Asynchronous**
  - For large payload sizes up to 1GB
  - Long processing times
  - Near-real time latency requirements
  - Request and responses are in Amazon S3
- **Batch**
  - Prediction for an entire dataset (multiple predictions)

# 6. Model Deployment Comparison

TODO TABLE

# 7. SageMaker Studio

- End-to-end ML development from a unified interface.
- Team collaboration.
- Tune and debug ML models.
- Deploy ML models.
- Automated workflows.

# 8. Data Wrangler

- Prepare tabular and image data for machine learning.
- Data preparation, transformation and feature engineering.
- Single interface for data selection, cleansing, exploration, visualization, and processing.
- SQL support.
- Data Quality tool.
- Offering 300+ pre-configured data transformations to prepare these different data modalities.

# 9. What are ML Features?

- Features are inputs to ML models used during training and used for inference.
- **Example**
  - **Music dataset:** Song ratings, listening duration, and listener demographics
- Important to have high quality features across our datasets in our company for re-use.

## 9.1. Feature Store

- Ingests features from a variety of sources.
- Ability to define the transformation of data into feature from within Feature Store.
- Can publish directly from SageMaker Data Wrangler into SageMaker Feature Store.
- Features are discoverable within SageMaker Studio.

# 10. Clarify

- Evaluate Foundation Models.
- Evaluating human-factors such as friendliness or humor.
- Leverage an AWS-managed team or bring our own employees.
- Use built-in datasets or bring our own dataset.
- Built-in metrics and algorithms.
- Part of **SageMaker Studio**.

## 10.1. Model Explainability

- A set of tools to help explain how machine learning (ML) models make predictions.
  -Understand model characteristics as a whole prior to deployment.
  -Debug predictions provided by the model after it's deployed.
  -Helps increase the trust and understanding of the model.
- **Example**
  - "Why did the model predict a negative outcome such as a loan rejection for a given applicant?"
  - "Why did the model make an incorrect prediction?"

## 10.2. Detect Bias (human)

- Ability to **detect and explain** biases in our **datasets and models**.
- Measure bias using statistical metrics.
- Specify input features and bias will be automatically detected.
  - https://noise.getoto.net/author/julien-simon/

## 10.3. Different kind of biases (definitions)

- **Sampling bias:** Sampling bias occurs when the training data does not represent the full population fairly, leading to a model that over-represents or disproportionately affects certain groups.
- **Measurement bias:** Measurement bias occurs when the tools or measurements used in data collection are flawed or skewed.
- Observer bias: Observer bias happens when the person collecting or interpreting the data has personal biases that affect the results.
- **Confirmation bias:** Confirmation bias is when individuals interpret or favor information that confirms their preconceptions. This is more applicable to human decision-making rather than automated model outputs.
- **Example:** An algorithm only flags people from specific ethnic groups, this is probably a sampling bias, and we need to perform data augmentation for imbalanced classes.

# 11. Ground Truth

- RLHF - **R**einforcement **L**earning from **H**uman **F**eedback.
  - Model review, customization and evaluation.
  - Align model to human preferences.
  - Reinforcement learning where human feedback is included in the "reward" function.
- **Human feedback for ML**
  - Creating or evaluating our models.
  - Data generation or annotation (create labels).
- **Reviewers:** Amazon Mechanical Turk workers, our employees, or third-party vendors.
- **SageMaker Ground Truth Plus:** Label Data.
  ![Amazon SageMaker - Ground Truth](/Images/Machine%20Learning/AmazonSageMakerGroundTruth.png)

# 12. ML Governance

- **SageMaker Model Cards**
  - Essential model information.
  - **Example:** Intended uses, risk ratings, and training details.
- **SageMaker Model Dashboard**
  - Centralized repository.
  - Information and insights for all models.
- **SageMaker Role Manager**
  - Define roles for personas.
  - **Example:** Data scientists, MLOps engineers.

## 12.1. Model Dashboard

- Centralized portal where we can view, search, and explore all of your models.
- **Example:** Track which models are deployed for inference.
- Can be accessed from the SageMaker Console.
- Helps us find models that violate thresholds you set for data quality, model quality, bias, explainability...

## 12.2. Model Monitor

- **Monitor the quality of your model in production:** Continuous or on-schedule.
- **Alerts for deviations in the model quality:** Fix data & retrain model.
- **Example:** Loan model starts giving loans to people who don't have the correct credit score (drift).

## 12.3. Model Registry

- Centralized repository allows you to track, manage, and version ML models.
- Catalog models, manage model versions, associate metadata with a model.
- Manage approval status of a model, automate model deployment, share models...
  https://docs.aws.amazon.com/sagemaker/latest/dg/mlflow-track-experiments-model-registration.html

# 13. Pipelines

- SageMaker Pipeline - A workflow that automates the process of building, training, and deploying a ML model.
- Continuous Integration and Continuous Delivery (CI/CD) service for Machine Learning.
- Help us easily build, train, test, and deploy 100s of models automatically.
- Iterate faster, reduce errors (no manual steps), repeatable mechanisms...
  https://aws.amazon.com/sagemaker/pipelines/
- Pipelines composed of Steps and each Step performs a specific task (e.g., data preprocessing, model training...).
- **Supported Step Types**
  - **Processing** - For data processing (e.g., feature engineering).
  - **Training** - For training a model.
  - **Tuning** - For hyperparameter tuning (e.g., Hyperparameter Optimization).
  - **AutoML** - To automatically train a model.
  - **Model** - To create or register a SageMaker model.
  - **ClarifyCheck** - Perform drift checks against baselines (Data bias, Model bias, Model explainability).
  - **QualityCheck** - Perform drift checks against baselines (Data quality, Model quality).
- For a full list check docs: https://docs.aws.amazon.com/sagemaker/latest/dg/build-andmanage-steps.html#build-and-manage-steps-types

# 14. JumpStart

- ML Hub to find pre-trained Foundation Model (FM), computer vision models, or natural language processing models.
- Large collection of models from Hugging Face, Databricks, Meta, Stability AI...
- Models can be fully customized for your data and use-case.
- Models are deployed on SageMaker directly (full control of deployment options).
- Pre-built ML solutions for demand forecasting, credit rate prediction, fraud detection and computer vision.

## 14.1. ML Hub

![alt](/Images/Machine%20Learning/AmazonSageMakerJumpStartMLHub.png)

## 14.2. ML Solutions

![alt](/Images/Machine%20Learning/AmazonSageMakerJumpStartMLSolutions.png)

# 15. Canvas

- Build ML models using a visual interface (no coding required).
- Access to ready-to-use models from **Bedrock** or **JumpStart**.
- Build your own custom model using **AutoML** powered by **SageMaker Autopilot**.
- Part of **SageMaker Studio**.
- Leverage **Data Wrangler** for data preparation.

## 15.1. Ready-to-use models

- Ready-to-use models from Amazon Rekognition, Amazon Comprehend, Amazon Textract.
- Makes it easy to build a full ML pipeline without writing code and leveraging various AWS AI Services.

# 16. MLFlow on Amazon SageMaker

- **MLFlow:** An open-source tool which helps ML teams manage the entire ML lifecycle.
- **MLFlow Tracking Servers**
  - Used to track runs and experiments.
  - Launch on SageMaker with a few clicks.
- Fully integrated with SageMaker (part of SageMaker Studio).

# 17. Summary

- **SageMaker:** End-to-end ML service.
- **SageMaker Automatic Model Tuning:** Tune hyperparameters.
- **SageMaker Deployment & Inference:** Real-time, serverless, batch, async.
- **SageMaker Studio:** Unified interface for SageMaker.
- **SageMaker Data Wrangler:** Explore and prepare datasets, create features.
- **SageMaker Feature Store:** Store features metadata in a central place.
- **SageMaker Clarify:** Compare models, explain model outputs, detect bias.
- **SageMaker Ground Truth:** RLHF, humans for model grading and data labeling.
- **SageMaker Model Cards:** ML model documentation.
- **SageMaker Model Dashboard:** View all your models in one place.
- **SageMaker Model Monitor:** Monitoring and alerts for your model.
- **SageMaker Model Registry:** Centralized repository to manage ML model versions.
- **SageMaker Pipelines:** CICD for Machine Learning.
- **SageMaker Role Manager:** Access control.
- **SageMaker JumpStart:** ML model hub & pre-built ML solutions.
- **SageMaker Canvas:** No-code interface for SageMaker.
- **MLFlow on SageMaker:** Use MLFlow tracking servers on AWS.

# 18. Extra Features

- **Network Isolation mode**
  - Run SageMaker job containers without any outbound internet access.
- Can't even access Amazon S3.
- **SageMaker DeepAR forecasting algorithm**
  - Used to forecast time series data.
  - Leverages Recurrent Neural Network (RNN).

# 19. Savings Plan

- **Amazon SageMaker is not covered** by Compute Savings Plans.
- Requires a **SageMaker-specific Savings Plan** for cost optimization.
- **Applies to various SageMaker components**
  - Training jobs.
  - Real-time inference.
  - Batch transform.
  - Studio notebooks.
- Offers **up to 64% savings** vs. On-Demand pricing.
- Best for **stable and predictable inference workloads**.
- Simplifies cost management with **dedicated, long-term pricing** (1- or 3-year commitment).
