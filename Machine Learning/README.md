# Machine Learning <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Why AWS AI Managed Services?](#1-why-aws-ai-managed-services)
- [2. Amazon Rekognition](#2-amazon-rekognition)
- [3. Amazon Transcribe](#3-amazon-transcribe)
- [4. Amazon Polly](#4-amazon-polly)
- [5. Amazon Translate](#5-amazon-translate)
- [6. Amazon Lex \& Connect](#6-amazon-lex--connect)
- [7. Amazon Comprehend](#7-amazon-comprehend)
- [8. Amazon SageMaker](#8-amazon-sagemaker)
- [9. Amazon Forecast](#9-amazon-forecast)
- [10. Amazon Kendra](#10-amazon-kendra)
- [11. Amazon Personalize](#11-amazon-personalize)
- [12. Amazon Textract](#12-amazon-textract)
- [13. Amazon's Hardware for AI](#13-amazons-hardware-for-ai)
  - [13.1. AWS Inferentia vs. AWS Trainium](#131-aws-inferentia-vs-aws-trainium)
- [14. Summary](#14-summary)

# 1. Why AWS AI Managed Services?

- AWS AI Services are pre-trained ML services for our use case.
- Responsiveness and Availability.
- **Redundancy and Regional Coverage:** Deployed across multiple Availability Zones and AWS regions.
- **Performance:** Specialized CPU and GPUs for specific use-cases for cost saving.
- **Token-based pricing:** Pay for what we use.
- **Provisioned throughput:** For predictable workloads, cost savings and predictable performance.

# 2. Amazon Rekognition

- **Amazon Rekognition makes it easy to add image and video analysis to our applications using proven, highly scalable, deep learning technology that requires no machine learning expertise to use.**
  - [Amazon Rekognition](Amazon%20Rekognition.md)

# 3. Amazon Transcribe

- **Amazon Transcribe is an AWS service that makes it easy for customers to convert speech-to-text.**
  - [Amazon Transcribe](Amazon%20Transcribe.md)

# 4. Amazon Polly

- **Amazon Polly is a service that turns text into lifelike speech.**
  - [Amazon Polly](Amazon%20Polly.md)

# 5. Amazon Translate

- **Amazon Translate is a neural machine translation service that delivers fast, high-quality, and affordable language translation.**
  - [Amazon Translate](Amazon%20Translate.md)

# 6. Amazon Lex & Connect

- **Amazon Lex is a service for building conversational interfaces into any application using voice and text. Lex provides the advanced deep learning functionalities of automatic speech recognition (ASR) for converting speech to text, and natural language understanding (NLU) to recognize the intent of the text, to enable we to build applications with highly engaging user experiences and lifelike conversational interactions.**
  - [Amazon Lex](Amazon%20Lex.md)

# 7. Amazon Comprehend

- **Amazon Comprehend is a natural language processing (NLP) service that uses machine learning to find meaning and insights in text.**
  - [Amazon Comprehend](Amazon%20Comprehend.md)

# 8. Amazon SageMaker

- **Amazon SageMaker is a fully managed service that provides every developer and data scientist with the ability to build, train, and deploy machine learning (ML) models quickly. SageMaker removes the heavy lifting from each step of the machine learning process to make it easier to develop high quality models.**
  [Amazon SageMaker](Amazon%20SageMaker.md)

# 9. Amazon Forecast

- Fully managed service that uses ML to deliver highly accurate forecasts.
- **Example:** Predict the future sales of a raincoat.
- 50% more accurate than looking at the data itself.
- Reduce forecasting time from months to hours.
- **Use cases:** Product Demand Planning, Financial Planning, Resource Planning, ...

# 10. Amazon Kendra

- **Amazon Kendra is a highly accurate and easy to use enterprise search service that's powered by machine learning.**
  - [Amazon Kendra](Amazon%20Kendra.md)

# 11. Amazon Personalize

- **Amazon Personalize is a machine learning service that makes it easy for developers to create individualized recommendations for customers using their applications.**
  - [Amazon Personalize](Amazon%20Personalize.md)

# 12. Amazon Textract

[Amazon Textract](Amazon%20Textract.md)

# 13. Amazon's Hardware for AI

- GPU-based EC2 Instances (P3, P4, P5..., G3...G6...)
- **AWS Trainium**
  - ML chip built to perform Deep Learning on 100B+ parameter models.
  - `Trn1` instance has for example 16 Trainium Accelerators.
  - 50% cost reduction when training a model.
- **AWS Inferentia**
  - ML chip built to deliver inference at high performance and low cost.
  - `Inf1`, `Inf2` instances are powered by AWS Inferentia.
  - Up to 4x throughput and 70% cost reduction.
- Trn & Inf have the **lowest environmental footprint**.

## 13.1. AWS Inferentia vs. AWS Trainium

| AWS Inferentia                               | AWS Trainium                                 |
| -------------------------------------------- | -------------------------------------------- |
| Optimized for **inference**                  | Optimized for **training**                   |
| Runs trained models efficiently              | Trains deep learning models                  |
| High throughput                              | High-performance training                    |
| Lower inference cost                         | Lower training cost                          |
| Used with **Amazon EC2 Inf1/Inf2** instances | Used with **Amazon EC2 Trn1/Trn2** instances |

# 14. Summary

- **Rekognition:** Face detection, labeling, celebrity recognition.
- **Transcribe:** Audio to text (ex: subtitles).
- **Polly:** Text to audio.
- **Translate:** Translations.
- **Lex:** Build conversational bots - chatbots.
- **Connect:** Cloud contact center.
- **Comprehend:** Natural language processing.
- **SageMaker:** Machine learning for every developer and data scientist.
- **Forecast:** Build highly accurate forecasts.
- **Kendra:** ML-powered search engine.
- **Personalize:** Real-time personalized recommendations.
- **Textract:** Detect text and data in documents.
