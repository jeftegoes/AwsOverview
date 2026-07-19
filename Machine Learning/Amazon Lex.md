# Amazon Lex <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Chatbot](#2-chatbot)
  - [2.1. Session Attributes](#21-session-attributes)
- [3. Amazon DynamoDB with Amazon Lex](#3-amazon-dynamodb-with-amazon-lex)

# 1. Introduction

- **Amazon Lex** (same technology that powers Alexa).
  - Automatic Speech Recognition (ASR) to convert speech to text.
  - Natural Language Understanding to recognize the intent of text, callers.
  - Helps build chatbots, call center bots.

# 2. Chatbot

- Build chatbots quickly for our applications using voice and text.
- **Example:** A chatbot that allows our customers to order pizzas or book a hotel.
- Supports multiple languages.
- Integration with AWS Lambda, Connect, Comprehend, Kendra.
- The bot automatically understands the user intent to invoke the correct Lambda function to "fulfill the intent".
- The bot will ask for "Slots" (input parameters) if necessary.

## 2.1. Session Attributes

- **Session attributes** are **key-value pairs** that store contextual information during a conversation, allowing an Amazon Lex chatbot to maintain context across multiple interactions.
- **Purpose**
  - Preserve conversation context.
  - Personalize responses.
  - Share information between intents.
  - Improve the conversational experience.
- **Example**
  - **User:** "I'd like to check in on **May 5th**."
    > Session attribute:
        ```text
        checkInDate = May 5th
        ```

# 3. Amazon DynamoDB with Amazon Lex

- **Amazon DynamoDB** is a **fully managed NoSQL database service** that provides **low-latency, scalable, and highly available** storage for structured and semi-structured data.
- Amazon Lex can use **Amazon DynamoDB** to **store and retrieve user information**, enabling personalized and context-aware conversations.
