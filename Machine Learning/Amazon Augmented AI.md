# Amazon Augmented AI (A2I) <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Amazon Augmented AI (A2I) vs. Amazon Mechanical Turk](#2-amazon-augmented-ai-a2i-vs-amazon-mechanical-turk)

# 1. Introduction

- Human oversight of Machine Learning predictions in production.
  - Can be your own employees, over 500,000 contractors from AWS, or AWS Mechanical Turk.
  - Some vendors are pre-screened for confidentiality requirements.
- The ML model can be built on AWS or elsewhere (SageMaker, Rekognition...).

TODO DIAGRAM

# 2. Amazon Augmented AI (A2I) vs. Amazon Mechanical Turk

| Amazon Augmented AI (A2I)               | Amazon Mechanical Turk (MTurk)              |
| --------------------------------------- | ------------------------------------------- |
| Human review workflow service           | Crowdsourcing marketplace                   |
| Adds **human review** to AI predictions | Provides **human workers** to perform tasks |
| Integrates with AI/ML services          | Can be used independently of AI             |
| Used when model confidence is low       | Used to label or process data               |
| Automates routing to human reviewers    | Humans manually complete assigned tasks     |
