# Amazon Lex <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Chatbot](#2-chatbot)
  - [2.1. Session Attributes](#21-session-attributes)

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
