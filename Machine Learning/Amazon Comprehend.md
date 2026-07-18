# Amazon Comprehend <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Custom Classification](#2-custom-classification)
- [3. Named Entity Recognition (NER)](#3-named-entity-recognition-ner)
- [4. Custom Entity Recognition](#4-custom-entity-recognition)
- [5. Use cases](#5-use-cases)
  - [5.1. Find documents about a subject](#51-find-documents-about-a-subject)
  - [5.2. Find out how customers feel about our products](#52-find-out-how-customers-feel-about-our-products)
  - [5.3. Discover what matters to our customers](#53-discover-what-matters-to-our-customers)
- [6. Amazon Comprehend Medical](#6-amazon-comprehend-medical)
- [7. Amazon Comprehend NLP Features](#7-amazon-comprehend-nlp-features)
- [8. Amazon Comprehend Batch Processing](#8-amazon-comprehend-batch-processing)

# 1. Introduction

- For **Natural Language Processing - NLP**.
- Fully managed and serverless service.
- Uses machine learning to find insights and relationships in text:
  - Language of the text.
  - Extracts key phrases, places, people, brands, or events.
  - Understands how positive or negative the text is.
  - Analyzes text using tokenization and parts of speech.
  - Automatically organizes a collection of text files by topic.
- **Sample use cases**
  - Analyze customer interactions (emails) to find what leads to a positive or negative experience.
  - Create and groups articles by topics that Comprehend will uncover.

# 2. Custom Classification

- Organize documents into categories (classes) that we define.
- **Example:** Categorize customer emails so that we can provide guidance based on the type of the customer request.
- Supports different document types (text, PDF, Word, images...).
- **Real-time Analysis:** Dingle document, synchronous.
- **Async Analysis:** Multiple documents (batch), Asynchronous.
  ![Amazon Comprehend - Custom Classification](/Images/Machine%20Learning/AmazonComprehendCustomClassification.png)

# 3. Named Entity Recognition (NER)

- NER - Extracts predefined, general-purpose entities like people, places, organizations, dates, and other standard categories, **from text**.

# 4. Custom Entity Recognition

- Analyze text for specific terms and noun-based phrases.
- Extract terms like policy numbers, or phrases that imply a customer escalation, anything specific to our business.
- Train the model with custom data such as a list of the entities and documents that contain them.
- Real-time or Async analysis.
  ![Amazon Comprehend - Custom Entity Recognition](/Images/Machine%20Learning/AmazonComprehendCustomEntityRecognition.png)

# 5. Use cases

## 5.1. Find documents about a subject

- Find the documents about a particular subject using Amazon Comprehend topic modeling.
- Scan a set of documents to determine the topics discussed, and to find the documents associated with each topic.
- We can specify the number of topics that Amazon Comprehend should return from the document set.

## 5.2. Find out how customers feel about our products

- If our company publishes a catalog, let Amazon Comprehend tell we what customers think of our products.
- Send each customer comment to the DetectSentiment operation and it will tell we whether customers feel positive, negative, neutral, or mixed about a product.

## 5.3. Discover what matters to our customers

- Use Amazon Comprehend topic modeling to discover the topics that our customers are talking about on our forums and message boards, then use entity detection to determine the people, places, and things that they associate with the topic.
- Use sentiment analysis to determine how our customers feel about a topic.

# 6. Amazon Comprehend Medical

- **Amazon Comprehend Medical** detects and returns useful information in unstructured clinical text:
  - Physician's notes.
  - Discharge summaries.
  - Test results.
  - Case notes.
- **Uses NLP to detect Protected Health Information (PHI)** - DetectPHI API.
- Store our documents in Amazon S3.
- Analyze real-time data with Kinesis Data Firehose.
- Use Amazon Transcribe to transcribe patient narratives into text that can be analyzed by Amazon Comprehend Medical.

# 7. Amazon Comprehend NLP Features

| Feature                  | Purpose                                                                           | Example                                                                                                     |
| ------------------------ | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Entity Recognition**   | Identifies named entities in text (people, organizations, locations, dates, etc.) | "John works at AWS in Seattle." -> **John (Person), AWS (Organization), Seattle (Location)**                |
| **Keyphrase Extraction** | Extracts the most important words or phrases from text                            | "Amazon Bedrock simplifies Generative AI development." -> **Amazon Bedrock**, **Generative AI development** |
| **Sentiment Analysis**   | Detects the emotional tone of text                                                | **Positive**, **Negative**, **Neutral**, or **Mixed**                                                       |
| **Text Classification**  | Categorizes documents into predefined labels                                      | Support ticket -> **Claims**, **Billing Issues**, **Technical Support**                                     |

# 8. Amazon Comprehend Batch Processing

- **Batch Processing** in Amazon Comprehend analyzes **large collections of documents stored in Amazon S3** asynchronously, making it ideal for bulk NLP tasks.
- **Purpose**
  - Analyze large document collections.
  - Process data asynchronously.
  - Scale automatically.
  - Reduce manual effort.
