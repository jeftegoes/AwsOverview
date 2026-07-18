# Amazon Rekognition <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Custom Labels](#2-custom-labels)
- [3. Content Moderation](#3-content-moderation)
- [4. Content Moderation API](#4-content-moderation-api)
- [5. Kinesis Use Case](#5-kinesis-use-case)
- [6. Amazon Rekognition Features](#6-amazon-rekognition-features)
- [7. Amazon Rekognition Face Indexing](#7-amazon-rekognition-face-indexing)

# 1. Introduction

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
    ![Amazon Rekognition](/Images/Machine%20Learning/AmazonRekognition.png)

# 2. Custom Labels

- **Examples:** Find our logo in social media posts, identify our products on stores shelves (National Football League - NFL - uses it to find their logo in pictures).
- **Label** our training images and upload them to Amazon Rekognition.
- Only needs a few hundred images or less.
- Amazon Rekognition creates a custom model on our images set.
- New subsequent images will be categorized the custom way we have defined.
  ![Amazon Rekognition - Custom Labels](/Images/Machine%20Learning/AmazonRekognitionCustomLabels.png)

# 3. Content Moderation

- Automatically detect inappropriate, unwanted, or offensive content.
- **Example:** Filter out harmful images in social media, broadcast media, advertising...
- Bring down human review to 1-5% of total content volume.
- Integrated with Amazon Augmented AI (Amazon A2I) for human review.
- **Custom Moderation Adaptors**
  - Extends Rekognition capabilities by providing our own labeled set of images.
  - Enhances the accuracy of Content Moderation or create a specific use case of Moderation.
    ![Amazon Rekognition - Content Moderation](/Images/Machine%20Learning/AmazonRekognitionContentModeration.png)

# 4. Content Moderation API

![Amazon Rekognition - Content Moderation API](/Images/Machine%20Learning/AmazonRekognitionContentModerationAPI.png)

# 5. Kinesis Use Case

![Kinesis Use Case](/Images/Machine%20Learning/AmazonRekognitionKinesisUseCase.png)

# 6. Amazon Rekognition Features

| Feature                   | Purpose                                                                        | Example Use Case                                                      |
| ------------------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| **Object Detection**      | Detects and locates objects within an image by returning their bounding boxes. | Detect empty shelves, vehicles, packages, or products.                |
| **Label Detection**       | Identifies objects, scenes, activities, and concepts in an image.              | Recognize shelves, products, groceries, furniture, or outdoor scenes. |
| **Face Recognition**      | Identifies or verifies known faces by comparing them with a face collection.   | Employee authentication, access control, customer identification.     |
| **Celebrity Recognition** | Identifies well-known public figures in images or videos.                      | Media analysis, entertainment, content indexing.                      |

# 7. Amazon Rekognition Face Indexing

- **Face Indexing** is an Amazon Rekognition feature that **detects, extracts, and stores facial features** in a face collection, enabling fast and accurate face searches and matching.
