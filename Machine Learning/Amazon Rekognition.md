# Amazon Rekognition <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Custom Labels](#2-custom-labels)
- [3. Content Moderation](#3-content-moderation)
- [4. Content Moderation API - Diagram](#4-content-moderation-api---diagram)
- [5. Kinesis Use Case](#5-kinesis-use-case)

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

TODO DIAGRAM

# 4. Content Moderation API - Diagram

TODO DIAGRAM

# 5. Kinesis Use Case

![Kinesis Use Case](/Images/Machine%20Learning/AmazonRekognitionKinesisUseCase.png)
