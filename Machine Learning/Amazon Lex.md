# Amazon Lex <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Chatbot](#2-chatbot)

# 1. Introduction

- **Amazon Lex:** (same technology that powers Alexa).
  - Automatic Speech Recognition (ASR) to convert speech to text.
  - Natural Language Understanding to recognize the intent of text, callers.
  - Helps build chatbots, call center bots.
- **Amazon Connect is a self-service, cloud-based contact center service that makes it easy for any business to deliver better customer service at lower cost. It does not provide speech-to-text conversion or natural language understanding.**
- **Amazon Connect**
  - Receive calls, create contact flows, cloud-based virtual contact center.
  - Can integrate with other CRM systems or AWS.
  - No upfront payments, 80% cheaper than traditional contact center solutions.

# 2. Chatbot

- Build chatbots quickly for our applications using voice and text.
- **Example:** A chatbot that allows our customers to order pizzas or book a hotel.
- Supports multiple languages.
- Integration with AWS Lambda, Connect, Comprehend, Kendra.
- The bot automatically understands the user intent to invoke the correct Lambda function to "fulfill the intent".
- The bot will ask for "Slots" (input parameters) if necessary.
