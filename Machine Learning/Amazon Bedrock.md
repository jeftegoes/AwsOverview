# Amazon Bedrock <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is Generative AI?](#1-what-is-generative-ai)
  - [1.1. Foundation Model](#11-foundation-model)
  - [1.2. Large Language Models (LLM)](#12-large-language-models-llm)
  - [1.3. Generative Language Models](#13-generative-language-models)
- [2. Introduction](#2-introduction)
- [3. Foundation Models](#3-foundation-models)
- [4. Amazon Bedrock Diagram](#4-amazon-bedrock-diagram)
- [5. Base Foundation Model](#5-base-foundation-model)
- [6. Amazon Titan vs. Llama vs. Claude vs. Stable Diffusion](#6-amazon-titan-vs-llama-vs-claude-vs-stable-diffusion)
- [7. Fine-Tuning a Model](#7-fine-tuning-a-model)
  - [7.1. Supervised Fine-Tuning](#71-supervised-fine-tuning)
  - [7.2. Reinforcement Fine-Tuning](#72-reinforcement-fine-tuning)
    - [7.2.1. Example](#721-example)
  - [7.3. Supervised Fine Tuning vs Reinforcement Fine Tuning](#73-supervised-fine-tuning-vs-reinforcement-fine-tuning)
  - [7.4. Distillation](#74-distillation)
  - [7.5. Fine-Tuning: Good to know](#75-fine-tuning-good-to-know)
  - [7.6. Fine-Tuning - Use cases](#76-fine-tuning---use-cases)
  - [7.7. Continued Pre-training](#77-continued-pre-training)
  - [7.8. Continued Pre-training vs Fine-tuning](#78-continued-pre-training-vs-fine-tuning)
- [8. Model Evaluation in Amazon Bedrock](#8-model-evaluation-in-amazon-bedrock)
  - [8.1. Automatic Evaluation](#81-automatic-evaluation)
  - [8.2. Benchmark Datasets](#82-benchmark-datasets)
  - [8.3. Human Evaluation](#83-human-evaluation)
  - [8.4. Automated Metrics to Evaluate an FM](#84-automated-metrics-to-evaluate-an-fm)
  - [8.5. Automated Model Evaluation](#85-automated-model-evaluation)
  - [8.6. Business Metrics to Evaluate a Model On](#86-business-metrics-to-evaluate-a-model-on)
- [9. RAG \& Knowledge Base](#9-rag--knowledge-base)
  - [9.1. Knowledge Bases for Amazon Bedrock](#91-knowledge-bases-for-amazon-bedrock)
  - [9.2. RAG Vector Databases](#92-rag-vector-databases)
  - [9.3. RAG Vector Databases by AWS](#93-rag-vector-databases-by-aws)
  - [9.4. RAG Data Sources](#94-rag-data-sources)
  - [9.5. Amazon Bedrock - RAG - Use Cases](#95-amazon-bedrock---rag---use-cases)
- [10. GenAI Concepts](#10-genai-concepts)
  - [10.1. Context Window](#101-context-window)
  - [10.2. Embeddings](#102-embeddings)
- [11. Guardrails](#11-guardrails)
- [12. Agents](#12-agents)
  - [12.1. Bedrock Agent Setup](#121-bedrock-agent-setup)
  - [12.2. Agent - Diagram](#122-agent---diagram)
  - [12.3. Amazon Bedrock \& CloudWatch](#123-amazon-bedrock--cloudwatch)
- [13. Pricing](#13-pricing)
- [14. Model Improvement Techniques Cost Order](#14-model-improvement-techniques-cost-order)
  - [14.1. Instruction-Based Fine-Tuning](#141-instruction-based-fine-tuning)
  - [14.2. Domain Adaptation](#142-domain-adaptation)
- [15. Cost savings](#15-cost-savings)
- [16. Security](#16-security)
  - [16.1. Bedrock must access an encrypted S3 bucket](#161-bedrock-must-access-an-encrypted-s3-bucket)
  - [16.2. Deploy SageMaker Model in your VPC](#162-deploy-sagemaker-model-in-your-vpc)
  - [16.3. Access Bedrock Model using an App in VPC](#163-access-bedrock-model-using-an-app-in-vpc)
  - [16.4. Analyze Bedrock access with CloudTrail](#164-analyze-bedrock-access-with-cloudtrail)
- [17. Response Length \& Penalties](#17-response-length--penalties)
  - [17.1. Response Length](#171-response-length)
  - [17.2. Penalties](#172-penalties)
  - [17.3. Stop Sequences](#173-stop-sequences)
- [18. Dynamic Prompt Engineering](#18-dynamic-prompt-engineering)
- [19. Amazon Bedrock Playground](#19-amazon-bedrock-playground)

# 1. What is Generative AI?

- Generative AI (Gen-AI) is a subset of Deep Learning
- Used to generate new data that is similar to the data it was trained on
  - Text.
  - Image.
  - Audio.
  - Code.
  - Video...
    ![What is Generative AI?](/Images/Machine%20Learning/WhatIsGenerativeAI.png)

## 1.1. Foundation Model

- To generate data, we must rely on a Foundation Model.
- Foundation Models are trained on a wide variety of input data.
- The models may cost tens of millions of dollars to train.
- **Example:** GPT-4o is the foundation model behind ChatGPT.
- There is a wide selection of Foundation Models from companies:
  - OpenAI.
  - Meta (Facebook).
  - Amazon.
  - Google.
  - Anthropic.
- Some foundation models are open-source (free: Meta, Google BERT) and others under a commercial license (OpenAI, Anthropic, etc...).
  ![Foundation Model](/Images/Machine%20Learning/FoundationModel.png)

## 1.2. Large Language Models (LLM)

- Type of AI designed to generate coherent human-like text.
- **One notable example:** GPT-4 (ChatGPT / Open AI).
- Trained on large corpus of text data.
- **Usually very big models**
  - Billions of parameters.
  - Trained on books, articles, websites, other textual data.
- **Can perform language-related tasks**
  - Translation, Summarization.
  - Question answering.
  - Content creation.

## 1.3. Generative Language Models

- We usually interact with the LLM by giving a prompt.
- Then, the model will leverage all the existing content it has learned from to generate new content.
- Non-deterministic: the generated text may be different for every user that uses the same prompt.
- The LLM generates a list of potential words alongside probabilities.
- An algorithm selects a word from that list.

# 2. Introduction

- Build Generative AI (Gen-AI) applications on AWS.
- Fully-managed service, no servers for you to manage.
- Keep control of your data used to train the model.
- Pay-per-use pricing model.
- Unified APIs.
- Leverage a wide array of foundation models.
- Out-of-the box features: RAG, LLM Agents...
- Security, Privacy, Governance and Responsible AI features.

# 3. Foundation Models

- Access to a wide range of Foundation Models (FM).
- Amazon Bedrock makes a copy of the FM, available only to us, which we can further fine-tune with our own data.
- **None of our data is used to train the FM.**

# 4. Amazon Bedrock Diagram

- TODO: DIAGRAM

# 5. Base Foundation Model

- How to choose?
  - Model types, performance requirements, capabilities, constraints, compliance.
  - Level of customization, model size, inference options, licensing agreements, context windows, latency.
  - Multimodal models (varied types of input and outputs).
- What's **Amazon Titan**?
  - High-performing Foundation Models from AWS.
  - Image, text, multimodal model choices via a fully-managed APIs.
  - Can be customized with our own data.
- Smaller models are more cost-effective.

# 6. Amazon Titan vs. Llama vs. Claude vs. Stable Diffusion

TODO TABLE TABLE

# 7. Fine-Tuning a Model

- Adapt a **copy** of a foundation model with **our own data**.
- Fine-tuning will change the **weights** of the base foundation model.
- **Training data must**
  - Adhere to a **specific format**.
  - **Be stored in Amazon S3.**
- **Note:** Not all models can be fine-tuned.
  ![Amazon Bedrock - Fine-Tuning a Model](/Images/Machine%20Learning/AmazonBedrockFineTuningModel.png)

## 7.1. Supervised Fine-Tuning

- Improves the performance of a Model on specific tasks.
- = further trained on a particular field or area of knowledge.
- Supervised Fine-Tuning uses **labeled examples** that are **input-output pairs**.
- **Labeled data**
  ```json
  {
    "prompt": "Who is Jefté Goes?",
    "completion": "Jefté Goes is an AWS Certified."
  }
  ```

## 7.2. Reinforcement Fine-Tuning

- Improves our FM model using **feedback-based learning**.
- We provide the input data (training data - prompts).
- We define a **Reward Function** to evaluate responses (output) and judge which responses are good.
  - Objective tasks => use AWS Lambda (python code).
  - Subjective tasks => use another model to "judge" by providing evaluation instructions.
- Model learns iteratively from **reward functions output scores** and will try to achieve high scores over time.

### 7.2.1. Example

- **Example:** Technical customer support chatbot
  - Sample customer prompt: "My app is running very slowly."
  - Judge model instructions: have empathy, and run diagnostics with the user:
    - **Response 1**
      - "Restart the app"
      - Comment: Helpful but superficial-Solves some cases-No diagnosis-No learning or trust built
      - Score: 5.0
    - **Response 2**
      - "That sounds frustrating. I can help you figure this out. Before we troubleshoot, can you tell me when it started and what you were doing at the time?"
      - Comment: Empathetic, diagnostic, efficient-Acknowledges user frustration-Gathers signal before acting-Leads to faster, better resolution
      - Score: 9.0
    - **Response 3**
      - "Please open a support ticket and attach logs A, B, and C."
      - Comment: Not helpful, creates friction-Pushes work to the user-Breaks conversational flow-Poor experience for a simple issue
      - Score: 2.0

## 7.3. Supervised Fine Tuning vs Reinforcement Fine Tuning

## 7.4. Distillation

- Making models smaller and faster.
- Up to 75% less expensive than original models.
- Decrease in accuracy, but potentially acceptable.
- Larger (teacher) model transfers knowledge to a smaller (student) model.
- We provide input data (ex: prompts).
- Produces a **lighter model** with similar behavior to the original.
- Focuses on efficiency, speed, and cost reduction.

## 7.5. Fine-Tuning: Good to know

- Re-training an FM requires a higher budget.
- Supervised Fine-Tuning is usually cheaper as computations are less intense and less data is usually required.
- It also requires experienced ML engineers to perform the task.
- We must prepare the data, do the fine-tuning, evaluate the model.
- Running a fine-tuned model is also more expensive.
  - **Option 1:** Run the custom model "on-demand" (price per token).
  - **Option 2:** Purchase provisioned throughput (billed per month).

## 7.6. Fine-Tuning - Use cases

- A chatbot designed with a particular persona or tone, or geared towards a specific purpose (e.g., assisting customers, crafting advertisements)
- Training using more up-to-date information than what the language model previously accessed
- Training with exclusive data (e.g., your historical emails or messages, records from customer service interactions)
- Targeted use cases (categorization, assessing accuracy).

## 7.7. Continued Pre-training

- Uses **unlabeled datasets**.
- Enhances the model's understanding of a specific domain.
- Updates model weights based on new knowledge.
- **Ideal for**
  - Internal company documents.
  - Industry-specific terminology.
  - Proprietary knowledge bases.
- Can be repeated as new data becomes available.

## 7.8. Continued Pre-training vs Fine-tuning

| Continued Pre-training                                           | Fine-tuning                                                                              |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| Self-supervised                                                  | Supervised                                                                               |
| Uses **unlabeled data**                                          | Uses **labeled data**                                                                    |
| Improves **domain knowledge**                                    | Improves **task-specific performance**                                                   |
| Teaches the model new information and context                    | Teaches the model desired input-output behavior                                          |
| Suitable for proprietary documents and industry-specific content | Suitable for classification, summarization, question answering, and other specific tasks |
| Focuses on expanding what the model knows                        | Focuses on improving how the model responds                                              |

# 8. Model Evaluation in Amazon Bedrock

- Compare multiple foundation models.
- Choose the best model for your application.
- Detect bias and fairness issues.
- Optimize model performance before production.

## 8.1. Automatic Evaluation

- Evaluate a model for quality control.
- **Built-in task types**
  - Text summarization.
  - Question and answer.
  - Text classification.
  - Open-ended text generation...
- Bring your own prompt dataset or use built-in curated prompt datasets.
- Scores are calculated automatically.
- Model scores are calculated using various statistical methods (e.g. BERTScore, F1...).

## 8.2. Benchmark Datasets

- Curated collections of data designed specifically at evaluating the performance of language models.
- Wide range of topics, complexities, linguistic phenomena.
- **Helpful to measure:** Accuracy, speed and efficiency, scalability.
- Some benchmarks datasets allow you to very quickly detect any kind of bias and potential discrimination against a group of people.
- We can also create our own benchmark dataset that is specific to your business.

TODO DIAGRAM

## 8.3. Human Evaluation

- Choose a work team to evaluate.
  - Employees of your company.
  - Subject-Matter Experts (SMEs).
- Define metrics and how to evaluate.
  - Thumbs up/down, ranking...
- Choose from Built-in task types (same as Automatic) or add a custom task.

TODO DIAGRAM

## 8.4. Automated Metrics to Evaluate an FM

- **ROUGE:** Recall-Oriented Understudy for Gisting Evaluation
  - Evaluating automatic summarization and machine translation systems.
  - ROUGE-N - measure the number of matching n-grams between reference and generated text.
  - ROUGE-L - longest common subsequence between reference and generated text.
- **BLEU:** Bilingual Evaluation Understudy
  - Evaluate the quality of generated text, especially for translations.
  - Considers both precision and penalizes too much brevity.
  - Looks at a combination of n-grams (1, 2, 3, 4).
- **BERTScore**
  - Semantic similarity between generated text.
  - Uses pre-trained BERT models (Bidirectional Encoder Representations from Transformers) to compare the contextualized embeddings of both texts and computes the cosine similarity between them.
  - Capable of capturing more nuance between the texts.
- **Perplexity:** How well the model predicts the next token (lower is better).

TODO DIAGRAM

## 8.5. Automated Model Evaluation

TODO DIAGRAM

## 8.6. Business Metrics to Evaluate a Model On

- **User Satisfaction:** Gather users' feedbacks and assess their satisfaction with the model responses (e.g., user satisfaction for an ecommerce platform).
- **Average Revenue Per User (ARPU):** Average revenue per user attributed to the Gen-AI app (e.g., monitor ecommerce user base revenue).
- **Cross-Domain Performance:** Measure the model's ability to perform cross different domains tasks (e.g., monitor multi-domain ecommerce platform).
- **Conversion Rate:** Generate recommended desired outcomes such as purchases (e.g., optimizing ecommerce platform for higher conversion rate)
- **Efficiency:** Evaluate the model's efficiency in computation, resource utilization... (e.g., improve production line efficiency).

# 9. RAG & Knowledge Base

- RAG = Retrieval-Augmented Generation.
- Allows a Foundation Model to reference a data source outside of its training data.
- Bedrock takes care of creating Vector Embeddings in the database of your choice based on your data.
  [Amazon Bedrock - RAG & Knowledge Base](/Images/Machine%20Learning/AmazonBedrockRAGKnowledgeBase.png)

## 9.1. Knowledge Bases for Amazon Bedrock

- With Knowledge Bases for Amazon Bedrock, you can give FMs and agents contextual information from our company's private data sources for RAG to deliver more relevant, accurate, and customized responses.
- Knowledge Bases for Amazon Bedrock takes care of the entire ingestion workflow of converting our documents into embeddings (vector) and storing the embeddings in a specialized vector database.
- Knowledge Bases for Amazon Bedrock supports popular databases for vector storage, including vector engine for Amazon OpenSearch Serverless, Pinecone, Redis Enterprise Cloud, Amazon Aurora (coming soon), and MongoDB (coming soon).
- If we do not have an existing vector database, Amazon Bedrock creates an OpenSearch Serverless vector store for us.

## 9.2. RAG Vector Databases

![Amazon Bedrock - RAG Vector Databases](/Images/Machine%20Learning/AmazonBedrockRAGVectorDatabases.png)

## 9.3. RAG Vector Databases by AWS

- **Amazon OpenSearch Service (Serverless & Managed Cluster):** Search & analytics database real time similarity queries, store millions of vector embeddings scalable index management, and fast nearest neighbor (kNN) search capability.
- **Amazon Aurora PostgreSQL:** Relational database, proprietary on AWS.
- **Amazon Neptune Analytics:** Graph database that enables high performance graph analytics and graph-based RAG (GraphRAG) solutions.
- **Amazon S3 Vectors:** Cost-effective and durable storage with sub-second query performance.

## 9.4. RAG Data Sources

- Amazon S3.
- Confluence.
- Microsoft SharePoint.
- Salesforce.
- Web pages (your website, your social media feed, etc...).
- More added over time...

## 9.5. Amazon Bedrock - RAG - Use Cases

- **Customer Service Chatbot**
  - **Knowledge Base:** Products, features, specifications, troubleshooting guides, and FAQs.
  - **RAG application:** Chatbot that can answer customer queries.
- **Legal Research and Analysis**
  - **Knowledge Base:** Laws, regulations, case precedents, legal opinions, and expert analysis.
  - **RAG Application:** Chatbot that can provide relevant information for specific legal queries.
- **Healthcare Question-Answering**
  - **Knowledge base:** Diseases, treatments, clinical guidelines, research papers, patients...
  - **RAG application:** Chatbot that can answer complex medical queries.

# 10. GenAI Concepts

- **Tokenization:** Conver ting raw text into a sequence of tokens.
  - **Word-based tokenization:** Text is split into individual words.
  - **Subword tokenization:** Some words can be split too (helpful for long words...).
- **Can experiment at:** https://platform.openai.com/tokenizer

## 10.1. Context Window

- The number of tokens an LLM can consider when generating text.
- The larger the context window, the more information and coherence.
- Large context windows require more memory and processing power.
  > **IMPORTANT!:** First factor to look at when considering a model.

## 10.2. Embeddings

- Create vectors (array of numerical values) out of text, images or audio.
- Vectors have a high dimensionality to capture many features for one input token, such as semantic meaning, syntactic role, sentiment.
- Embedding models can power search applications.
  ![Gen AI - Concepts Embeddings](/Images/Machine%20Learning/GenAIConceptsEmbeddings.png)

# 11. Guardrails

- Control the interaction between users and Foundation Models (FMs).
- Filter undesirable and harmful content.
- Remove Personally Identifiable Information (PII).
- Enhanced privacy.
- Reduce hallucinations.
- Ability to create multiple Guardrails and monitor and analyze user inputs that can violate the Guardrails.
  - ![Amazon Bedrock - Guardrails](/Images/Machine%20Learning/AmazonBedrockGuardrails.png)

# 12. Agents

- Manage and carry out **various multi-step tasks** related to infrastructure provisioning, application deployment, and operational activities.
- **Task coordination:** Perform tasks in the correct order and ensure information is passed correctly between tasks.
- Agents are configured to perform specific pre-defined action groups.
- Integrate with other systems, services, databases and API to exchange data or initiate actions.
- Leverage RAG to retrieve information when necessary.

## 12.1. Bedrock Agent Setup

- TODO: DIAGRAM

## 12.2. Agent - Diagram

![alt](/Images/Machine%20Learning/AmazonBedrockAgent.png)

## 12.3. Amazon Bedrock & CloudWatch

- **Model Invocation Logging**
  - Send logs of all invocations to Amazon CloudWatch and S3.
  - Can include text, images and embeddings.
  - Analyze further and build alerting thanks to CloudWatch Logs Insights.
    ![Amazon Bedrock - Model Invocation Logging](/Images/Machine%20Learning/AmazonBedrockCloudWatchLogging.png)
- **CloudWatch Metrics**
  - Published metrics from Bedrock to CloudWatch.
    - Including ContentFilteredCount, which helps to see if Guardrails are functioning.
  - Can build CloudWatch Alarms on top of Metrics.
    ![Amazon Bedrock - CloudWatch Metrics](/Images/Machine%20Learning/AmazonBedrockCloudWatchMetrics.png)

# 13. Pricing

- **On-Demand**
  - Pay-as-you-go (no commitment).
  - **Text Models:** Charged for every input/output token processed.
  - **Embedding Models:** Charged for every input token processed.
  - **Image Models:** Charged for every image generated.
  - Works with Base Models only.
- **Batch**
  - Multiple predictions at a time (output is a single file in Amazon S3).
  - Can provide discounts of up to 50%.
- **Provisioned Throughput**
  - Purchase Model units for a certain time (1 month, 6 months...).
  - Throughput - max. number of input/output tokens processed per minute.
  - Works with Base, Fine-tuned, and Custom Models.

# 14. Model Improvement Techniques Cost Order

1. **Prompt Engineering $**
   - No model training needed (no additional computation or fine-tuning).
2. **Retrieval Augmented Generation (RAG) $$**
   - Uses external knowledge (FM doesn't need to "know everything", less complex).
   - No FM changes (no additional computation or fine-tuning).
3. **Instruction-based Fine-tuning $$$**
   - FM is fine-tuned with specific instructions (requires additional computation).
4. **Domain Adaptation Fine-tuning $$$$**
   - Model is trained on a domain-specific dataset (requires intensive computation).

## 14.1. Instruction-Based Fine-Tuning

- **Instruction-Based Fine-Tuning** is a model customization technique that trains a **pre-trained Foundation Model (FM)** using **labeled prompt–response pairs** written as instructions.
- Unlike prompt engineering, this process **modifies the model's weights**.
- **Training Data Format**
  ```text
  Instruction (Prompt)  ->  Expected Response
  ```
- **Example**
  - **Prompt**
    > Translate the following sentence to French:
    > "Hello, how are you?"
  - **Response**
    > Bonjour, comment allez-vous ?

## 14.2. Domain Adaptation

- **Domain Adaptation** is a machine learning technique that adapts a model trained on one **source domain** to perform well on a **different but related target domain**.

# 15. Cost savings

- **On-Demand:** Great for unpredictable workloads, no long-term commitment.
- **Batch:** Provides up to 50% discounts.
- **Provisioned Throughput:** (usually) not a cost-saving measure, great to "reserve" capacity.
- **Temperature, Top K, Top P:** No impact on pricing.
- **Model size:** Usually a smaller model will be cheaper (varies based on providers).
- **Number of Input and Output Tokens:** Main driver of cost.

# 16. Security

- **IAM with Bedrock**
  - Implement identity verification and resource-level access control.
  - Define roles and permissions to access Bedrock resources (e.g., data scientists).
- **GuardRails for Bedrock**
  - Restrict specific topics in a GenAI application.
  - Filter harmful content.
  - Ensure compliance with safety policies by analyzing user inputs.
- **CloudTrail with Bedrock:** Analyze API calls made to Amazon Bedrock.
- **Config with Bedrock:** Look at configuration changes within Bedrock.
- **PrivateLink with Bedrock:** Keep all API calls to Bedrock within the private VPC.

## 16.1. Bedrock must access an encrypted S3 bucket

TODO DIAGRAM

## 16.2. Deploy SageMaker Model in your VPC

TODO DIAGRAM

## 16.3. Access Bedrock Model using an App in VPC

TODO DIAGRAM

## 16.4. Analyze Bedrock access with CloudTrail

TODO DIAGRAM

# 17. Response Length & Penalties

## 17.1. Response Length

- Controls the size of the generated response.
  - Set **minimum** or **maximum** number of tokens.
  - Prevents responses from being too short or too long.

## 17.2. Penalties

- **Reduce undesirable output by penalizing**
  - Long responses.
  - Repeated tokens or phrases.
  - Frequently used tokens.
  - Specific token types.

## 17.3. Stop Sequences

- Define one or more character sequences that tell the model **when to stop generating text**.

# 18. Dynamic Prompt Engineering

- **Dynamic Prompt Engineering** customizes the input prompt sent to a Large Language Model (LLM) based on user context, without changing the model itself.
- **Example**
  | User | Dynamic Prompt |
  |------|----------------|
  | Child | "Respond using simple, friendly language." |
  | Teen | "Respond in an engaging way with relatable examples." |
  | Adult | "Provide a concise and informative response." |
  | Senior | "Provide a clear, respectful response with detailed explanations." |

# 19. Amazon Bedrock Playground

- **Amazon Bedrock Playground** is an interactive environment for **testing, comparing, and refining prompts and model parameters** before integrating Foundation Models into an application.
- **Key Features**
  - Interactive prompt testing.
  - Real-time parameter adjustment (e.g., temperature, Top P, Top K).
  - Immediate response preview.
  - Compare Foundation Models.
  - Experiment without writing application code.
