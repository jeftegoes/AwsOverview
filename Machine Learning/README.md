# Machine Learning <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Amazon Rekognition](#1-amazon-rekognition)
  - [1.1. Content Moderation](#11-content-moderation)
- [2. Amazon Transcribe](#2-amazon-transcribe)
- [3. Amazon Polly](#3-amazon-polly)
  - [3.1. Lexicon \& SSML](#31-lexicon--ssml)
- [4. Amazon Translate](#4-amazon-translate)
- [5. Amazon Lex \& Connect](#5-amazon-lex--connect)
- [6. Amazon Comprehend](#6-amazon-comprehend)
  - [6.1. Medical](#61-medical)
- [7. Amazon SageMaker](#7-amazon-sagemaker)
- [8. Amazon Forecast](#8-amazon-forecast)
- [9. Amazon Kendra](#9-amazon-kendra)
- [10. Amazon Personalize](#10-amazon-personalize)
- [11. Amazon Textract](#11-amazon-textract)
- [12. Summary](#12-summary)

# 1. Amazon Rekognition

- **Amazon Rekognition makes it easy to add image and video analysis to your applications using proven, highly scalable, deep learning technology that requires no machine learning expertise to use.**
- Find **objects, people, text, scenes in images and videos** using ML.
- **Facial analysis** and **facial search** to do user verification, people counting.
- Create a database of "familiar faces" or compare against celebrities.
- **Use cases**
  - Labeling.
  - Content Moderation.
  - Text Detection.
  - Face Detection and Analysis (gender, age range, emotions...).
  - Face Search and Verification.
  - Celebrity Recognition.
  - Pathing (ex: for sports game analysis).

## 1.1. Content Moderation

- Detect content that is inappropriate, unwanted, or offensive (image and videos).
- Used in social media, broadcast media, advertising, and e-commerce situations to create a safer user experience.
- **Set a Minimum Confidence Threshold for items that will be flagged.**
- Flag sensitive content for manual review in Amazon Augmented AI (A2I).
- Help comply with regulations.

# 2. Amazon Transcribe

- **Amazon Transcribe is an AWS service that makes it easy for customers to convert speech-to-text.**
- Automatically **convert speech to text**.
- Uses a **deep learning process called automatic speech recognition (ASR)** to convert speech to text quickly and accurately.
- **Automatically remove Personally Identifiable Information (PII) using Redaction.**
- **Supports Automatic Language Identification for multi-lingual audio.**
- **Use cases**
  - Transcribe customer service calls.
  - Automate closed captioning and subtitling.
  - Generate metadata for media assets to create a fully searchable archive.

# 3. Amazon Polly

- **Amazon Polly is a service that turns text into lifelike speech.**
- Turn text into lifelike speech using deep learning.
- Allowing you to create applications that talk.

## 3.1. Lexicon & SSML

- Customize the pronunciation of words with Pronunciation lexicons.
  - Stylized words: St3ph4ne => "Stephane".
  - Acronyms: AWS => "Amazon Web Services".
- Upload the lexicons and use them in the SynthesizeSpeech operation.
- Generate speech from plain text or from documents marked up with **Speech Synthesis Markup Language (SSML)** - enables more customization.
  - Emphasizing specific words or phrases.
  - Using phonetic pronunciation.
  - Including breathing sounds, whispering.
  - Using the Newscaster speaking style.

# 4. Amazon Translate

- **Amazon Translate is a neural machine translation service that delivers fast, high-quality, and affordable language translation.**
- Natural and accurate **language translation**.
- Amazon Translate allows you to **localize content** - such as websites and applications - for **international users**, and to easily translate large volumes of text efficiently.

# 5. Amazon Lex & Connect

- **Amazon Lex is a service for building conversational interfaces into any application using voice and text. Lex provides the advanced deep learning functionalities of automatic speech recognition (ASR) for converting speech to text, and natural language understanding (NLU) to recognize the intent of the text, to enable you to build applications with highly engaging user experiences and lifelike conversational interactions.**
- **Amazon Lex:** (same technology that powers Alexa).
  - Automatic Speech Recognition (ASR) to convert speech to text.
  - Natural Language Understanding to recognize the intent of text, callers.
  - Helps build chatbots, call center bots.
- **Amazon Connect is a self-service, cloud-based contact center service that makes it easy for any business to deliver better customer service at lower cost. It does not provide speech-to-text conversion or natural language understanding.**
- **Amazon Connect:**
  - Receive calls, create contact flows, cloud-based virtual contact center.
  - Can integrate with other CRM systems or AWS.
  - No upfront payments, 80% cheaper than traditional contact center solutions.

# 6. Amazon Comprehend

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

## 6.1. Medical

- Amazon Comprehend Medical detects and returns useful information in unstructured clinical text:
  - Physician's notes.
  - Discharge summaries.
  - Test results.
  - Case notes.
- **Uses NLP to detect Protected Health Information (PHI)** - DetectPHI API.
- Store your documents in Amazon S3, analyze real-time data with Kinesis Data Firehose, or use Amazon Transcribe to transcribe patient narratives into text that can be analyzed by Amazon Comprehend Medical.

# 7. Amazon SageMaker

- **Amazon SageMaker is a fully managed service that provides every developer and data scientist with the ability to build, train, and deploy machine learning (ML) models quickly. SageMaker removes the heavy lifting from each step of the machine learning process to make it easier to develop high quality models.**
- Fully managed service for developers / data scientists to build ML models.
- Typically, difficult to do all the processes in one place + provision servers.
- Machine learning process (simplified): predicting your exam score.

# 8. Amazon Forecast

- Fully managed service that uses ML to deliver highly accurate forecasts.
- Example: predict the future sales of a raincoat.
- 50% more accurate than looking at the data itself.
- Reduce forecasting time from months to hours.
- **Use cases:** Product Demand Planning, Financial Planning, Resource Planning, ...

# 9. Amazon Kendra

- **Amazon Kendra is a highly accurate and easy to use enterprise search service that's powered by machine learning.**
- Fully managed **document search service** powered by Machine Learning.
- Extract answers from within a document (text, pdf, HTML, PowerPoint, MS Word, FAQs...).
- Natural language search capabilities.
- Learn from user interactions/feedback to promote preferred results **(Incremental Learning)**.
- Ability to manually fine-tune search results (importance of data, freshness, custom, ...).

# 10. Amazon Personalize

- **Amazon Personalize is a machine learning service that makes it easy for developers to create individualized recommendations for customers using their applications.**
- Fully managed ML-service to build apps with real-time personalized recommendations.
- Example: Personalized product recommendations/re-ranking, customized direct marketing.
  - Example: User bought gardening tools, provide recommendations on the next one to buy.
- Same technology used by Amazon.com.
- Integrates into existing websites, applications, SMS, email marketing systems, ...
- Implement in days, not months (you don't need to build, train, and deploy ML solutions).
- **Use cases:** retail stores, media and entertainment...

# 11. Amazon Textract

- Automatically extracts text, handwriting, and data from any scanned documents using AI and ML.
- Extract data from forms and tables.
- Read and process any type of document (PDFs, images, ...).
- **Use cases**
  - Financial Services (e.g., invoices, financial reports).
  - Healthcare (e.g., medical records, insurance claims).
  - Public Sector (e.g., tax forms, ID documents, passports).

# 12. Summary

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
