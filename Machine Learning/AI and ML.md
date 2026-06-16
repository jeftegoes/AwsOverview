# IA and ML <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is Artificial Intelligence (AI)?](#1-what-is-artificial-intelligence-ai)
  - [1.1. Use Cases](#11-use-cases)
- [2. AI Components](#2-ai-components)
- [3. What is Machine Learning (ML)?](#3-what-is-machine-learning-ml)
  - [3.1. AI != ML](#31-ai--ml)
- [4. What is Deep Learning (DL)?](#4-what-is-deep-learning-dl)
- [5. Neural Networks - How do they work?](#5-neural-networks---how-do-they-work)

# 1. What is Artificial Intelligence (AI)?

- AI is a broad field for the development of intelligent systems capable of performing tasks that typically require human intelligence:
  - Perception.
  - Reasoning.
  - Learning.
  - Problem solving.
  - Decision-making.
- Umbrella-term for various techniques

## 1.1. Use Cases

- Computer Vision
- Fraud Detection
- Facial Recognition
- Intelligent Document Processing (IDP)

# 2. AI Components

- **Data Layer:** Collect vast amount of data.
- **ML Framework and Algorithm Layer:** Data scientists and engineer work together to understand use cases, requirements, and frameworks that can solve them.
- **Model Layer:** Implement a model and train it, we have the structure, the parameters and functions, optimizer function.
- **Application Layer:** How to serve the model, and its capabilities for your users.

# 3. What is Machine Learning (ML)?

- ML is a type of AI for building methods that allow machines to learn.
- **Data** is leveraged to improve computer performance on a set of task.
- Make predictions based on data used to train the model.
- No explicit programming of rules.

## 3.1. AI != ML

- **Ex:** MYCIN Expert System
  - System developed in 1970s to diagnose patients based on reported symptoms and medical test results.
  - Collection of over 500 rules.
  - Simple yes/no or textual questions.
  - It provides a list of culprit bacteria ranked from high to low based on the probability of diagnosis, the reason behind the diagnosis, and a potential dosage for the cure.
  - Never really used in production as personal computers didn't exist yet.

# 4. What is Deep Learning (DL)?

- Uses neurons and synapses (like our brain) to train a model.
- Process more complex patterns in the data than traditional ML.
- Deep Learning because there's more than one layer of learning.
- **Ex:** Computer Vision - image classification, object detection, image segmentation.
- **Ex:** Natural Language Processing (NLP) - text classification, sentiment analysis, machine translation, language generation.
- Large amount of input data.
- Requires GPU (Graphical Processing Unit).

# 5. Neural Networks - How do they work?

- Nodes (tiny units) are connected together.
- Nodes are organized in layers.
- When the neural network sees a lot of data, it identifies patterns and changes the connections between the nodes.
- Nodes are "talking" to each other, by passing on (or not) data to the next layer.
- The math and parameters tuning behind it is beyond the level of this course.
- Neural networks may have billions of nodes.
