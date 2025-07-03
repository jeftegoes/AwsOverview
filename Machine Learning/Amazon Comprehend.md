# Amazon Comprehend <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
  - [1.1. Use cases](#11-use-cases)
    - [1.1.1. Find documents about a subject](#111-find-documents-about-a-subject)
    - [1.1.2. Find out how customers feel about your products](#112-find-out-how-customers-feel-about-your-products)
    - [1.1.3. Discover what matters to your customers](#113-discover-what-matters-to-your-customers)
- [2. Medical](#2-medical)

# 1. Introduction

- **Amazon Comprehend is a natural language processing (NLP) service that uses machine learning to find meaning and insights in text.**
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

## 1.1. Use cases

### 1.1.1. Find documents about a subject

- Find the documents about a particular subject using Amazon Comprehend topic modeling.
- Scan a set of documents to determine the topics discussed, and to find the documents associated with each topic.
- We can specify the number of topics that Amazon Comprehend should return from the document set.

### 1.1.2. Find out how customers feel about your products

- If your company publishes a catalog, let Amazon Comprehend tell you what customers think of your products.
- Send each customer comment to the DetectSentiment operation and it will tell you whether customers feel positive, negative, neutral, or mixed about a product.

### 1.1.3. Discover what matters to your customers

- Use Amazon Comprehend topic modeling to discover the topics that your customers are talking about on your forums and message boards, then use entity detection to determine the people, places, and things that they associate with the topic.
- Use sentiment analysis to determine how your customers feel about a topic.

# 2. Medical

- **Amazon Comprehend Medical** detects and returns useful information in unstructured clinical text:
  - Physician's notes.
  - Discharge summaries.
  - Test results.
  - Case notes.
- **Uses NLP to detect Protected Health Information (PHI)** - DetectPHI API.
- Store your documents in Amazon S3, analyze real-time data with Kinesis Data Firehose, or use Amazon Transcribe to transcribe patient narratives into text that can be analyzed by Amazon Comprehend Medical.
