# Amazon Bedrock <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Foundation Models](#2-foundation-models)
- [3. Amazon Bedrock Diagram](#3-amazon-bedrock-diagram)
- [4. Base Foundation Model](#4-base-foundation-model)
- [5. Amazon Titan vs. Llama vs. Claude vs. Stable Diffusion](#5-amazon-titan-vs-llama-vs-claude-vs-stable-diffusion)
- [6. Fine-Tuning a Model](#6-fine-tuning-a-model)
  - [6.1. Supervised Fine-Tuning](#61-supervised-fine-tuning)
  - [6.2. Reinforcement Fine-Tuning](#62-reinforcement-fine-tuning)
    - [6.2.1. Example](#621-example)
  - [6.3. Supervised Fine Tuning vs Reinforcement Fine Tuning](#63-supervised-fine-tuning-vs-reinforcement-fine-tuning)
  - [6.4. Distillation](#64-distillation)
  - [6.5. Fine-Tuning: Good to know](#65-fine-tuning-good-to-know)
  - [6.6. Fine-Tuning - Use cases](#66-fine-tuning---use-cases)
- [7. Automatic Evaluation](#7-automatic-evaluation)
  - [7.1. Note on Benchmark Datasets](#71-note-on-benchmark-datasets)
  - [7.2. Human Evaluation](#72-human-evaluation)
  - [7.3. Automated Metrics to Evaluate an FM](#73-automated-metrics-to-evaluate-an-fm)
  - [7.4. Automated Model Evaluation](#74-automated-model-evaluation)
  - [7.5. Business Metrics to Evaluate a Model On](#75-business-metrics-to-evaluate-a-model-on)
- [8. RAG \& Knowledge Base](#8-rag--knowledge-base)
  - [8.1. Knowledge Bases for Amazon Bedrock](#81-knowledge-bases-for-amazon-bedrock)
  - [8.2. RAG Vector Databases](#82-rag-vector-databases)
  - [8.3. RAG Vector Databases by AWS](#83-rag-vector-databases-by-aws)
  - [8.4. RAG Data Sources](#84-rag-data-sources)
  - [8.5. Amazon Bedrock - RAG - Use Cases](#85-amazon-bedrock---rag---use-cases)
- [9. GenAI Concepts](#9-genai-concepts)
  - [9.1. Context Window](#91-context-window)
  - [9.2. Embeddings](#92-embeddings)
- [10. Guardrails](#10-guardrails)
- [11. Agents](#11-agents)
  - [11.1. Bedrock Agent Setup](#111-bedrock-agent-setup)
  - [11.2. Agent - Diagram](#112-agent---diagram)
  - [11.3. Amazon Bedrock \& CloudWatch](#113-amazon-bedrock--cloudwatch)
- [12. Pricing](#12-pricing)
- [13. Model Improvement Techniques Cost Order](#13-model-improvement-techniques-cost-order)
- [14. Cost savings](#14-cost-savings)
- [15. Security](#15-security)
  - [15.1. Bedrock must access an encrypted S3 bucket](#151-bedrock-must-access-an-encrypted-s3-bucket)
  - [15.2. Deploy SageMaker Model in your VPC](#152-deploy-sagemaker-model-in-your-vpc)
  - [15.3. Access Bedrock Model using an App in VPC](#153-access-bedrock-model-using-an-app-in-vpc)
  - [15.4. Analyze Bedrock access with CloudTrail](#154-analyze-bedrock-access-with-cloudtrail)

# 1. Introduction

- Build Generative AI (Gen-AI) applications on AWS.
- Fully-managed service, no servers for you to manage.
- Keep control of your data used to train the model.
- Pay-per-use pricing model.
- Unified APIs.
- Leverage a wide array of foundation models.
- Out-of-the box features: RAG, LLM Agents...
- Security, Privacy, Governance and Responsible AI features.

# 2. Foundation Models

- Access to a wide range of Foundation Models (FM).
- Amazon Bedrock makes a copy of the FM, available only to us, which we can further fine-tune with our own data.
- **None of our data is used to train the FM.**

# 3. Amazon Bedrock Diagram

- TODO: DIAGRAM

# 4. Base Foundation Model

- How to choose?
  - Model types, performance requirements, capabilities, constraints, compliance.
  - Level of customization, model size, inference options, licensing agreements, context windows, latency.
  - Multimodal models (varied types of input and outputs).
- What's **Amazon Titan**?
  - High-performing Foundation Models from AWS.
  - Image, text, multimodal model choices via a fully-managed APIs.
  - Can be customized with our own data.
- Smaller models are more cost-effective.

# 5. Amazon Titan vs. Llama vs. Claude vs. Stable Diffusion

TODO TABLE TABLE

# 6. Fine-Tuning a Model

- Adapt a copy of a foundation model with our own data.
- Fine-tuning will change the weights of the base foundation model.
- **Training data must**
  - Adhere to a specific format.
  - Be stored in Amazon S3.
- **Note:** Not all models can be fine-tuned.

TODO: diagram

## 6.1. Supervised Fine-Tuning

- Improves the performance of a Model on specific tasks.
- = further trained on a particular field or area of knowledge.
- Supervised Fine-Tuning uses that are labeled examples output pairs.
- **Labeled data**
  ```json
  {
      Labeled Data
      "prompt": "Who is Jefté Goes?",
      "completion": "Jefté Goes is an AWS Certified."
  }
  ```

## 6.2. Reinforcement Fine-Tuning

- Improves our FM model using **feedback-based learning**.
- We provide the input data (training data - prompts).
- We define a Reward Function to evaluate responses (output) and judge which responses are good.
  - Objective tasks => use AWS Lambda (python code).
  - Subjective tasks => use another model to "judge" by providing evaluation instructions.
- Model learns iteratively from reward functions output scores and will try to achieve high scores over time.

### 6.2.1. Example

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

## 6.3. Supervised Fine Tuning vs Reinforcement Fine Tuning

## 6.4. Distillation

- Making models smaller and faster.
- Up to 75% less expensive than original models.
- Decrease in accuracy, but potentially acceptable.
- Larger (teacher) model transfers knowledge to a smaller (student) model.
- You provide input data (ex: prompts).
- Produces a **lighter model** with similar behavior to the original.
- Focuses on efficiency, speed, and cost reduction.

## 6.5. Fine-Tuning: Good to know

- Re-training an FM requires a higher budget.
- Supervised Fine-Tuning is usually cheaper as computations are less intense and less data is usually required.
- It also requires experienced ML engineers to perform the task.
- You must prepare the data, do the fine-tuning, evaluate the model.
- Running a fine-tuned model is also more expensive.
  - **Option 1:** Run the custom model "on-demand" (price per token).
  - **Option 2:** Purchase provisioned throughput (billed per month).

## 6.6. Fine-Tuning - Use cases

- A chatbot designed with a particular persona or tone, or geared towards a specific purpose (e.g., assisting customers, crafting advertisements)
- Training using more up-to-date information than what the language model previously accessed
- Training with exclusive data (e.g., your historical emails or messages, records from customer service interactions)
- Targeted use cases (categorization, assessing accuracy).

# 7. Automatic Evaluation

- Evaluate a model for quality control.
- **Built-in task types**
  - Text summarization.
  - Question and answer.
  - Text classification.
  - Open-ended text generation...
- Bring your own prompt dataset or use built-in curated prompt datasets.
- Scores are calculated automatically.
- Model scores are calculated using various statistical methods (e.g. BERTScore, F1...).

## 7.1. Note on Benchmark Datasets

- Curated collections of data designed specifically at evaluating the performance of language models.
- Wide range of topics, complexities, linguistic phenomena.
- **Helpful to measure:** Accuracy, speed and efficiency, scalability.
- Some benchmarks datasets allow you to very quickly detect any kind of bias and potential discrimination against a group of people.
- You can also create your own benchmark dataset that is specific to your business.

## 7.2. Human Evaluation

- Choose a work team to evaluate.
  - Employees of your company.
  - Subject-Matter Experts (SMEs).
- Define metrics and how to evaluate.
  - Thumbs up/down, ranking...
- Choose from Built-in task types (same as Automatic) or add a custom task.

## 7.3. Automated Metrics to Evaluate an FM

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

## 7.4. Automated Model Evaluation

TODO DIAGRAM

## 7.5. Business Metrics to Evaluate a Model On

- **User Satisfaction** - gather users' feedbacks and assess their satisfaction with the model responses (e.g., user satisfaction for an ecommerce platform).
- **Average Revenue Per User (ARPU)** - average revenue per user attributed to the Gen-AI app (e.g., monitor ecommerce user base revenue).
- **Cross-Domain Performance** - measure the model's ability to perform cross different domains tasks (e.g., monitor multi-domain ecommerce platform).
- **Conversion Rate** - generate recommended desired outcomes such as purchases (e.g., optimizing ecommerce platform for higher conversion rate)
- **Efficiency** - evaluate the model's efficiency in computation, resource utilization... (e.g., improve production line efficiency).

# 8. RAG & Knowledge Base

- RAG = Retrieval-Augmented Generation.
- Allows a Foundation Model to reference a data source outside of its training data.
- Bedrock takes care of creating Vector Embeddings in the database of your choice based on your data.
  [Amazon Bedrock - RAG & Knowledge Base](/Images/Machine%20Learning/AmazonBedrockRAGKnowledgeBase.png)

## 8.1. Knowledge Bases for Amazon Bedrock

- With Knowledge Bases for Amazon Bedrock, you can give FMs and agents contextual information from your company's private data sources for RAG to deliver more relevant, accurate, and customized responses.
- Knowledge Bases for Amazon Bedrock takes care of the entire ingestion workflow of converting your documents into embeddings (vector) and storing the embeddings in a specialized vector database.
- Knowledge Bases for Amazon Bedrock supports popular databases for vector storage, including vector engine for Amazon OpenSearch Serverless, Pinecone, Redis Enterprise Cloud, Amazon Aurora (coming soon), and MongoDB (coming soon).
- If we do not have an existing vector database, Amazon Bedrock creates an OpenSearch Serverless vector store for us.

## 8.2. RAG Vector Databases

![Amazon Bedrock - RAG Vector Databases](/Images/Machine%20Learning/AmazonBedrockRAGVectorDatabases.png)

## 8.3. RAG Vector Databases by AWS

- **Amazon OpenSearch Service (Serverless & Managed Cluster):** Search & analytics database real time similarity queries, store millions of vector embeddings scalable index management, and fast nearest neighbor (kNN) search capability.
- **Amazon Aurora PostgreSQL:** Relational database, proprietary on AWS.
- **Amazon Neptune Analytics:** Graph database that enables high performance graph analytics and graph-based RAG (GraphRAG) solutions.
- **Amazon S3 Vectors:** Cost-effective and durable storage with sub-second query performance.

## 8.4. RAG Data Sources

- Amazon S3.
- Confluence.
- Microsoft SharePoint.
- Salesforce.
- Web pages (your website, your social media feed, etc...).
- More added over time...

## 8.5. Amazon Bedrock - RAG - Use Cases

- **Customer Service Chatbot**
  - **Knowledge Base:** Products, features, specifications, troubleshooting guides, and FAQs.
  - **RAG application:** Chatbot that can answer customer queries.
- **Legal Research and Analysis**
  - **Knowledge Base:** Laws, regulations, case precedents, legal opinions, and expert analysis.
  - **RAG Application:** Chatbot that can provide relevant information for specific legal queries.
- **Healthcare Question-Answering**
  - **Knowledge base:** Diseases, treatments, clinical guidelines, research papers, patients...
  - **RAG application:** Chatbot that can answer complex medical queries.

# 9. GenAI Concepts

- **Tokenization:** Conver ting raw text into a sequence of tokens.
  - **Word-based tokenization:** Text is split into individual words.
  - **Subword tokenization:** Some words can be split too (helpful for long words...).
- **Can experiment at:** https://platform.openai.com/tokenizer

## 9.1. Context Window

- The number of tokens an LLM can consider when generating text.
- The larger the context window, the more information and coherence.
- Large context windows require more memory and processing power.
- First factor to look at when considering a model.

## 9.2. Embeddings

- Create vectors (array of numerical values) out of text, images or audio.
- Vectors have a high dimensionality to capture many features for one input token, such as semantic meaning, syntactic role, sentiment.
- Embedding models can power search applications.

# 10. Guardrails

- Control the interaction between users and Foundation Models (FMs).
- Filter undesirable and harmful content.
- Remove Personally Identifiable Information (PII).
- Enhanced privacy.
- Reduce hallucinations.
- Ability to create multiple Guardrails and monitor and analyze user inputs that can violate the Guardrails.
  - ![Amazon Bedrock - Guardrails](/Images/Machine%20Learning/AmazonBedrockGuardrails.png)

# 11. Agents

- Manage and carry out various multi-step tasks related to infrastructure provisioning, application deployment, and operational activities.
- **Task coordination:** Perform tasks in the correct order and ensure information is passed correctly between tasks.
- Agents are configured to perform specific pre-defined action groups.
- Integrate with other systems, services, databases and API to exchange data or initiate actions.
- Leverage RAG to retrieve information when necessary.

## 11.1. Bedrock Agent Setup

- TODO: DIAGRAM

## 11.2. Agent - Diagram

- TODO: DIAGRAM

## 11.3. Amazon Bedrock & CloudWatch

- **Model Invocation Logging**
  - Send logs of all invocations to Amazon CloudWatch and S3.
  - Can include text, images and embeddings.
  - Analyze further and build alerting thanks to CloudWatch Logs Insights.
- **CloudWatch Metrics**
  - Published metrics from Bedrock to CloudWatch.
    - Including ContentFilteredCount, which helps to see if Guardrails are functioning.
  - Can build CloudWatch Alarms on top of Metrics.

# 12. Pricing

- **On-Demand**
  - Pay-as-you-go (no commitment).
  - Text Models - Charged for every input/output token processed.
  - Embedding Models - Charged for every input token processed.
  - Image Models - Charged for every image generated.
  - Works with Base Models only.
- **Batch**
  - Multiple predictions at a time (output is a single file in Amazon S3).
  - Can provide discounts of up to 50%.
- **Provisioned Throughput**
  - Purchase Model units for a certain time (1 month, 6 months...).
  - Throughput - max. number of input/output tokens processed per minute.
  - Works with Base, Fine-tuned, and Custom Models.

# 13. Model Improvement Techniques Cost Order

1. **Prompt Engineering $**
   - No model training needed (no additional computation or fine-tuning)
2. **Retrieval Augmented Generation (RAG) $$**
   - Uses external knowledge (FM doesn't need to "know everything", less complex)
   - No FM changes (no additional computation or fine-tuning)
3. **Instruction-based Fine-tuning $$$**
   - FM is fine-tuned with specific instructions (requires additional computation)
4. **Domain Adaptation Fine-tuning $$$$**
   - Model is trained on a domain-specific dataset (requires intensive computation).

# 14. Cost savings

- **On-Demand:** Great for unpredictable workloads, no long-term commitment.
- **Batch:** Provides up to 50% discounts.
- **Provisioned Throughput:** (usually) not a cost-saving measure, great to "reserve" capacity.
- **Temperature, Top K, Top P:** No impact on pricing.
- **Model size:** Usually a smaller model will be cheaper (varies based on providers).
- **Number of Input and Output Tokens:** Main driver of cost.

# 15. Security

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

## 15.1. Bedrock must access an encrypted S3 bucket

TODO DIAGRAM

## 15.2. Deploy SageMaker Model in your VPC

TODO DIAGRAM

## 15.3. Access Bedrock Model using an App in VPC

TODO DIAGRAM

## 15.4. Analyze Bedrock access with CloudTrail

TODO DIAGRAM
