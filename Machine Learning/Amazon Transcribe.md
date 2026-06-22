# Amazon Transcribe <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Improving Accuracy](#2-improving-accuracy)
- [3. Toxicity Detection](#3-toxicity-detection)
- [4. Amazon Transcribe and Amazon Athena](#4-amazon-transcribe-and-amazon-athena)
- [5. Amazon Transcribe Medical](#5-amazon-transcribe-medical)

# 1. Introduction

- Automatically **convert speech to text**.
- Uses a **deep learning process called automatic speech recognition (ASR)** to convert speech to text quickly and accurately.
- **Automatically remove Personally Identifiable Information (PII) using Redaction.**
- **Supports Automatic Language Identification for multi-lingual audio.**
- **Use cases**
  - Transcribe customer service calls.
  - Automate closed captioning and subtitling.
  - Generate metadata for media assets to create a fully searchable archive.

# 2. Improving Accuracy

- Allows Transcribe to capture domain-specific or non standard terms (e.g., technical words, acronyms, jargon...).s
- **Custom Vocabularies (for words)**
  - Add specific words, phrases, domain-specific terms.
  - Good for brand names, acronyms...
  - Increase recognition of a new word by providing hints (such as pronunciation..).
- **Custom Language Models (for context)**
  - Train Transcribe model on our own domain-specific text data.
  - Good for transcribing large volumes of domain-specific speech.
  - Learn the context associated with a given word.
- **Note:** Use both for the highest transcription accuracy.

# 3. Toxicity Detection

- ML-powered, voice-based toxicity detection capability.
- **Leverages speech cues:** Tone and pitch, and text-based cues.
- **Toxicity categories:** Sexual harassment, hate speech, threat, abuse, profanity, insult, and graphic...

# 4. Amazon Transcribe and Amazon Athena

![Amazon Transcribe and Amazon Athena](/Images/Machine%20Learning/AmazonTranscribeAmazonAthenaIntegration.png)

# 5. Amazon Transcribe Medical

- Automatically convert medical-related speech to text (HIPAA compliant).
- Ability to transcribes medical terminologies such as:
  - Medicine names.
  - Procedures.
  - Conditions and diseases.
- Supports both real-time (microphone) and batch (upload files) transcriptions.
- **Use cases**
  - Voice applications that enable physicians to dictate medical notes.
  - Transcribe phone calls that report on drug safety and side effects.
