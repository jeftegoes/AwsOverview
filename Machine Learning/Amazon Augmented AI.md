# Amazon Augmented AI (A2I) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Amazon A2I vs. Amazon Mechanical Turk vs. Amazon SageMaker Ground Truth](#2-amazon-a2i-vs-amazon-mechanical-turk-vs-amazon-sagemaker-ground-truth)

# 1. Introduction

- Human oversight of Machine Learning predictions in production.
  - Can be your own employees, over 500,000 contractors from AWS, or AWS Mechanical Turk.
  - Some vendors are pre-screened for confidentiality requirements.
- The ML model can be built on AWS or elsewhere (SageMaker, Rekognition...).

TODO DIAGRAM

# 2. Amazon A2I vs. Amazon Mechanical Turk vs. Amazon SageMaker Ground Truth

| Feature                             | Amazon Augmented AI (A2I)                     | Amazon Mechanical Turk (MTurk)                    | Amazon SageMaker Ground Truth                      |
| ----------------------------------- | --------------------------------------------- | ------------------------------------------------- | -------------------------------------------------- |
| **Primary Purpose**                 | Human review of AI predictions                | Crowdsourcing human workforce                     | Build labeled datasets for ML                      |
| **Role in ML Lifecycle**            | **After deployment**                          | Before or during training                         | **Before training**                                |
| **Human Involvement**               | Human reviewers                               | Human workers                                     | Human labelers (optional)                          |
| **Main Function**                   | Reviews low-confidence predictions            | Performs Human Intelligence Tasks (HITs)          | Creates high-quality labeled data                  |
| **Typical Use Cases**               | Compliance, quality assurance, fraud review   | Data labeling, surveys, transcription, moderation | Image, text, and video labeling for model training |
| **Integrates with ML Services**     | Yes                                           | Not required                                      | Yes                                                |
| **Can Use Mechanical Turk Workers** | Yes                                           | N/A                                               | Yes                                                |
| **Supports Automation**             | Automatically routes predictions to reviewers | Manual task completion                            | Automatic labeling + human verification            |
| **Example**                         | Review suspicious fraud transactions          | Label 100,000 images                              | Create a labeled dataset for image classification  |
