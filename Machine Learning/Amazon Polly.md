# Amazon Polly <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Advanced Features](#2-advanced-features)
- [3. Speech Synthesis Markup Language (SSML)](#3-speech-synthesis-markup-language-ssml)
  - [3.1. What It Controls](#31-what-it-controls)
  - [3.2. Common Use Cases](#32-common-use-cases)
  - [3.4. Benefits](#34-benefits)

# 1. Introduction

- Turn text into lifelike speech using deep learning.
- Allowing we to create applications that talk.
  ![Amazon Polly](/Images/Machine%20Learning/AmazonPolly.png)

# 2. Advanced Features

- **Lexicons**
  - Define how to read certain specific pieces of text.
  - AWS => "Amazon Web Services".
  - W3C => "World Wide Web Consortium".
- **SSML - Speech Synthesis Markup Language**
  - Markup for our text to indicate how to pronounce it.
  - **Example:** "Hello, `<break>` how are you?"
- **Voice engine:** Generative, long-form, neural, standard...
- **Speech mark**
  - Encode where a sentence/word starts or ends in the audio.
  - Helpful for lip-syncing or highlight words as they're spoken.

# 3. Speech Synthesis Markup Language (SSML)

- **Speech Synthesis Markup Language (SSML)** is an XML-based markup language used to control **how text is spoken** by text-to-speech (TTS) systems, such as **Amazon Polly**.

## 3.1. What It Controls

- Speech rate (speed).
- Volume.
- Pitch.
- Pauses.
- Pronunciation.
- Emphasis.
- Emotional speaking style _(supported by some voices)_.

## 3.2. Common Use Cases

- Interactive Voice Response (IVR) systems
- Virtual assistants
- Customer service bots
- Accessibility applications
- Audiobooks
- **Example**
  ```xml
  <speak>
    <prosody rate="slow" pitch="+5%">
      Welcome to AWS Support.
    </prosody>
    <break time="1s"/>
    How can I help you today?
  </speak>
  ```

## 3.4. Benefits

- Produces more natural-sounding speech.
- Improves the user experience.
- Enables dynamic adjustment of speech for different scenarios.
- Supports customized voice output.
