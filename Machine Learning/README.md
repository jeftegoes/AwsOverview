# Machine Learning <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Amazon Rekognition](#1-amazon-rekognition)
- [2. Amazon Transcribe](#2-amazon-transcribe)
- [3. Amazon Polly](#3-amazon-polly)
  - [3.1. Lexicon \& SSML](#31-lexicon--ssml)
- [4. Amazon Translate](#4-amazon-translate)
- [5. Amazon Lex \& Connect](#5-amazon-lex--connect)
- [6. Amazon Comprehend](#6-amazon-comprehend)
- [7. Amazon SageMaker](#7-amazon-sagemaker)
- [8. Amazon Forecast](#8-amazon-forecast)
- [9. Amazon Kendra](#9-amazon-kendra)
- [10. Amazon Personalize](#10-amazon-personalize)
- [11. Amazon Textract](#11-amazon-textract)
- [12. Summary](#12-summary)

# 1. Amazon Rekognition

[Amazon Rekognition](Amazon%20Rekognition.md)

# 2. Amazon Transcribe

[Amazon Transcribe](Amazon%20Transcribe.md)

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

[Amazon Comprehend](Amazon%20Comprehend.md)

# 7. Amazon SageMaker

[Amazon SageMaker](Amazon%20SageMaker.md)

# 8. Amazon Forecast

- Fully managed service that uses ML to deliver highly accurate forecasts.
- **Example:** Predict the future sales of a raincoat.
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
- **Example:** Personalized product recommendations/re-ranking, customized direct marketing.
  - **Example:** User bought gardening tools, provide recommendations on the next one to buy.
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
