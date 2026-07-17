# IA and ML <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is Artificial Intelligence (AI)?](#1-what-is-artificial-intelligence-ai)
  - [1.1. Use Cases](#11-use-cases)
- [2. AI Components](#2-ai-components)
- [3. What is Machine Learning (ML)?](#3-what-is-machine-learning-ml)
  - [3.1. AI != ML](#31-ai--ml)
- [4. What is Deep Learning (DL)?](#4-what-is-deep-learning-dl)
  - [4.1. Model Training in Deep Learning](#41-model-training-in-deep-learning)
  - [4.2. How It Works](#42-how-it-works)
  - [4.3. Key Concepts](#43-key-concepts)
  - [4.4. Best Practices](#44-best-practices)
- [5. Neural Networks - How do they work?](#5-neural-networks---how-do-they-work)
- [6. What is Generative AI (Gen-AI)?](#6-what-is-generative-ai-gen-ai)
- [7. Transformer Models](#7-transformer-models)
  - [7.1. What is the Transformer Model? (LLM)](#71-what-is-the-transformer-model-llm)
  - [7.2. Key Concepts](#72-key-concepts)
- [8. Diffusion Models (ex: Stable Diffusion)](#8-diffusion-models-ex-stable-diffusion)
- [9. Multimodal Models (ex: GPT-4o)](#9-multimodal-models-ex-gpt-4o)
- [10. Humans are a mix of AI](#10-humans-are-a-mix-of-ai)
- [11. Generative Adversarial Network (GAN)](#11-generative-adversarial-network-gan)
  - [11.1. How It Works](#111-how-it-works)
- [12. ML Terms](#12-ml-terms)
- [13. Training Data](#13-training-data)
  - [13.1. Labeled vs. Unlabeled Data](#131-labeled-vs-unlabeled-data)
  - [13.2. Structured Data](#132-structured-data)
  - [13.3. Unstructured Data](#133-unstructured-data)
- [14. Pre-trained Model](#14-pre-trained-model)
- [15. ML Algorithms](#15-ml-algorithms)
  - [15.1. Supervised Learning](#151-supervised-learning)
    - [15.1.1. Classification](#1511-classification)
    - [15.1.2. Regression](#1512-regression)
    - [15.1.3. Training vs. Validation vs. Test Set](#1513-training-vs-validation-vs-test-set)
    - [15.1.4. Feature Engineering](#1514-feature-engineering)
      - [15.1.4.1. On Structured Data](#15141-on-structured-data)
      - [15.1.4.2. On Unstructured Data](#15142-on-unstructured-data)
  - [15.2. Unsupervised Learning](#152-unsupervised-learning)
    - [15.2.1. Clustering Technique](#1521-clustering-technique)
    - [15.2.2. Association Rule Learning Technique](#1522-association-rule-learning-technique)
    - [15.2.3. Anomaly Detection Technique](#1523-anomaly-detection-technique)
    - [15.2.4. Dimensionality Reduction](#1524-dimensionality-reduction)
  - [15.3. Semi-supervised Learning](#153-semi-supervised-learning)
  - [15.4. Self-Supervised Learning](#154-self-supervised-learning)
    - [15.4.1. Intuitive example](#1541-intuitive-example)
  - [15.5. Transfer Learning](#155-transfer-learning)
- [16. What is Reinforcement Learning (RL)?](#16-what-is-reinforcement-learning-rl)
  - [16.1. How Does Reinforcement Learning Work?](#161-how-does-reinforcement-learning-work)
  - [16.2. Reinforcement Learning in Action](#162-reinforcement-learning-in-action)
  - [16.3. Applications of Reinforcement Learning](#163-applications-of-reinforcement-learning)
- [17. What is RLHF?](#17-what-is-rlhf)
  - [17.1. How does RLHF work?](#171-how-does-rlhf-work)
  - [17.2. RLHF Process](#172-rlhf-process)
- [18. Model Fit](#18-model-fit)
- [19. Bias and Variance](#19-bias-and-variance)
- [20. Model Evaluation Metrics](#20-model-evaluation-metrics)
  - [20.1. Binary Classification](#201-binary-classification)
  - [20.2. Confusion Matrix](#202-confusion-matrix)
  - [20.3. AUC-ROC](#203-auc-roc)
  - [20.4. Regressions Metrics](#204-regressions-metrics)
- [21. Inferencing](#21-inferencing)
  - [21.1. Inferencing at the Edge](#211-inferencing-at-the-edge)
- [22. Phases of Machine Learning Project](#22-phases-of-machine-learning-project)
  - [22.1. Phase details](#221-phase-details)
  - [22.2. EDA - Exploratory Data Analysis](#222-eda---exploratory-data-analysis)
  - [22.3. Phases of Machine Learning Project](#223-phases-of-machine-learning-project)
- [23. Hyperparameter Tuning](#23-hyperparameter-tuning)
  - [23.1. Important Hyperparameters](#231-important-hyperparameters)
- [24. Model Parameters](#24-model-parameters)
- [25. Data Augmentation](#25-data-augmentation)
- [26. What to do if overfitting?](#26-what-to-do-if-overfitting)
- [27. When is Machine Learning NOT appropriate?](#27-when-is-machine-learning-not-appropriate)
- [28. Model evaluation vs Model inference](#28-model-evaluation-vs-model-inference)
- [29. Rule-Based System](#29-rule-based-system)
- [30. Deterministic vs. Probabilistic Models](#30-deterministic-vs-probabilistic-models)
  - [30.1. Deterministic Models](#301-deterministic-models)
  - [30.2. Probabilistic Models](#302-probabilistic-models)
  - [30.3. Hybrid Models](#303-hybrid-models)
- [31. CNN vs. RNN](#31-cnn-vs-rnn)
  - [31.1. CNN (Convolutional Neural Network)](#311-cnn-convolutional-neural-network)
  - [31.2. RNN (Recurrent Neural Network)](#312-rnn-recurrent-neural-network)

# 1. What is Artificial Intelligence (AI)?

- AI is a broad field for the development of intelligent systems capable of performing tasks that typically require human intelligence:
  - Perception.
  - Reasoning.
  - Learning.
  - Problem solving.
  - Decision-making.
- Umbrella-term for various techniques
  ![Artificial Intelligence](/Images/Machine%20Learning/ArtificialIntelligence.png)

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

## 4.1. Model Training in Deep Learning

- Model training is the process of teaching a neural network to make accurate predictions by learning from large datasets.

## 4.2. How It Works

1. Initialize the neural network.
2. Feed training data into the model.
3. Calculate the prediction error (loss).
4. Update weights and biases using **gradient descent** (or another optimizer).
5. Repeat the process over multiple **epochs** until performance improves.

## 4.3. Key Concepts

- **Weights & Biases:** Parameters the model learns.
- **Loss Function:** Measures prediction error.
- **Gradient Descent:** Optimizes weights to minimize loss.
- **Epoch:** One complete pass through the training dataset.
- **Hyperparameter Tuning:** Adjusts settings (e.g., learning rate, batch size, epochs) to improve performance.

## 4.4. Best Practices

- Prepare and clean the data.
- Use validation data to evaluate performance.
- Tune hyperparameters.
- Ensure the model generalizes well to unseen data.

# 5. Neural Networks - How do they work?

- Nodes (tiny units) are connected together.
- Nodes are organized in layers.
- When the neural network sees a lot of data, it identifies patterns and changes the connections between the nodes.
- Nodes are "talking" to each other, by passing on (or not) data to the next layer.
- The math and parameters tuning behind it is beyond the level of this course.
- Neural networks may have billions of nodes.
  ![Neural Networks - How do they work?](/Images/Machine%20Learning/NeuralNetworksHowDoTheyWork.png)

# 6. What is Generative AI (Gen-AI)?

- Subset of Deep Learning.
- Multi-purpose foundation models backed by neural networks.
- They can be fine-tuned if necessary to better fit our use-cases.
  ![Gen-AI](/Images/Machine%20Learning/GenAI.png)

# 7. Transformer Models

## 7.1. What is the Transformer Model? (LLM)

- It is a neural network architecture.
- Able to process a sentence as a whole instead of word by word.
- Faster and more efficient text processing (less training time).
- It gives relative importance to specific words in a sentence (more coherent sentences).
- **Transformer-based LLMs**
  - Powerful models that can understand and generate human-like text.
  - Trained on vast amounts of text data from the internet, books, and other sources, and learn patterns and relationships between words and phrases.
  - **Example:** Google BERT, OpenAI ChatGPT.
  - (ChatGPT = Chat Generative Pretrained Transformer).
    ![What is The Transformer Model](/Images/Machine%20Learning/WhatIsTheTransformerModel.png)

## 7.2. Key Concepts

- Uses **self-attention** to understand relationships between words.
- Implements **contextual embeddings**, where a word's meaning depends on its context.
- Captures long-range dependencies regardless of word position.
- Uses **positional encoding** to preserve word order.
- Follows an **encoder-decoder architecture** for processing and generating sequences.

# 8. Diffusion Models (ex: Stable Diffusion)

- **Training:** Forward diffusion process.
- **Generating:** Reverse diffusion process "a cat with a computer".
  ![Diffusion Models](/Images/Machine%20Learning/DiffusionModels.png)

# 9. Multimodal Models (ex: GPT-4o)

- Does NOT rely on a single type of input (text, or images, or audio only).
- Does NOT create a single type of output.
- **Example:** A multimodal can take a mix of audio, image and text and output a mix of video, text for example.
  ![Multimodal Modelss](/Images/Machine%20Learning/MultimodalModels.png)

# 10. Humans are a mix of AI

- Sometimes we know "if this happens, then do that" (AI).
- Sometimes we've seen a lot of similar things before, and we classify them (Machine Learning).
- Sometimes we haven't seen something before, but we have "learned" a lot of similar concepts, so we can make a decision (Deep Learning).
- Sometimes, we get creative, and based on what we've learned, we can generate content: Gen AI.

# 11. Generative Adversarial Network (GAN)

- A **Generative Adversarial Network (GAN)** is a deep learning model that generates **realistic synthetic data** while preserving the statistical characteristics of the original dataset.

## 11.1. How It Works

- A GAN consists of two neural networks:
  - **Generator** – Creates synthetic data.
  - **Discriminator** – Determines whether the data is real or synthetic.
- These networks compete against each other, improving the quality of the generated data over time.

# 12. ML Terms

- **GPT (Generative Pre-trained Transformer):** Generate human text or computer code based on input prompts.
- **BERT (Bidirectional Encoder Representations from Transformers):** Similar intent to GPT, but reads the text in two directions.
- **RNN (Recurrent Neural Network):** Meant for sequential data such as time-series or text, useful in speech recognition, time-series prediction.
- **ResNet (Residual Network):** Deep Convolutional Neural Network (CNN) used for image recognition tasks, object detection, facial recognition.
- **SVM (Support Vector Machine):** ML algorithm for classification and regression.
- **WaveNet:** Model to generate raw audio waveform, used in Speech Synthesis.
- **GAN (Generative Adversarial Network):** Models used to generate synthetic data such as images, videos or sounds that resemble the training data.
  - Helpful for data augmentation.
- **XGBoost (Extreme Gradient Boosting):** An implementation of gradient boosting.

# 13. Training Data

- To train our model we must have good data.
- Garbage in => Garbage out.
- Most critical stage to build a good model.
- Several options to model our data, which will impact the types of algorithms we can use to train our models.
- Labeled vs. Unlabeled Data.
- Structured vs. Unstructured Data.

## 13.1. Labeled vs. Unlabeled Data

- **Labeled Data**
  - Data includes both input features and corresponding output labels
  - **Example:** Dataset with images of animals where each image is labeled with the corresponding animal type (e.g., cat, dog)
  - **Use case:** **Supervised Learning**, where the model is trained to map inputs to known outputs
- **Unlabeled Data**
  - Data includes only input features without any output labels.
  - **Example:** A collection of images without any associated labels.
  - **Use case:** **Unsupervised Learning**, where the model tries to find patterns or structures in the data.

## 13.2. Structured Data

- Data is organized in a structured format, often in rows and columns (like Excel).
- **Tabular Data**
  - Data is arranged in a table with rows representing records and columns representing features.
  - **Example**
  - TODO: example
- **Time Series Data**
  - Data points collected or recorded at successive points in time.
  - **Example:** Stock prices recorded daily over a year.
    TODO: example

## 13.3. Unstructured Data

- Data that doesn't follow a specific structure and is often text-heavy or multimedia content.
- **Text Data**
  - Unstructured text such as articles, social media posts, or customer reviews.
  - **Example:** A collection of product reviews from an e-commerce site.
- **Image Data**
  - Data in the form of images, which can vary widely in format and content.
  - Example: Images used for object recognition tasks.

# 14. Pre-trained Model

- A **pre-trained model** is a model that has already been trained on a **large, general-purpose dataset** to learn broad patterns and features.
  - Instead of starting from scratch, it can be **fine-tuned** for a specific task using a smaller, task-specific dataset.

# 15. ML Algorithms

## 15.1. Supervised Learning

- Learn a mapping function that can predict the output for new unseen input data.
- **Needs labeled data:** Very powerful, but difficult to perform on millions of datapoints.

### 15.1.1. Classification

- Used to predict the categorical label of input data.
- The output variable is discrete, which means it falls into a specific category or class.
- **Use cases:** Scenarios where decisions or predictions need to be made between distinct categories (fraud, image classification, customer retention, diagnostics).
- **Examples**
  - **Binary Classification:** Classify emails as "spam" or "not spam".
  - **Multi-class Classification:** Classify animals in a zoo as "mammal," "bird," "reptile".
  - **Multi-label Classification:** Assign multiple labels to a movie, like "action" and "comedy.
- **Key algorithm:** K-nearest neighbors (kNN) model.

### 15.1.2. Regression

- Used to predict a numeric value based on input data.
- The output variable is continuous, meaning it can take any value within a range.
- **Use cases:** Used when the goal is to predict a quantity or a real value.
- **Examples**
  - **Predicting House Prices:** Based on features like size, location, and number of bedrooms.
  - **Stock Price Prediction:** Predicting the future price of a stock based on historical data and other features.
  - **Weather Forecasting:** Predicting temperatures based on historical weather data.

### 15.1.3. Training vs. Validation vs. Test Set

- **Training Set**
  - Used to train the model.
  - **Percentage:** Typically, 60-80% of the dataset.
  - **Example:** 800 labeled images from a dataset of 1000 images.
- **Validation Set (optional)**
  - Used to tune model parameters and validate performance.
  - **Percentage:** Typically, 10-20% of the dataset.
  - **Example:** 100 labeled images for hyperparameter tuning (tune the settings of the algorithm to make it more efficient).
- **Test Set**
  - Used to evaluate the final model performance.
  - **Percentage:** Typically, 10-20% of the dataset.
  - **Example:** 100 labeled images to test the model's accuracy.
    ![Training vs. Validation vs. Test Set](/Images/Machine%20Learning/TrainingValidationTestSet.png)

### 15.1.4. Feature Engineering

- The process of using domain knowledge to select and transform raw data into meaningful features.
- Helps enhancing the performance of machine learning models.
- **Techniques**
  - **Feature Selection:** Selecting a subset of relevant features, like choosing important predictors in a regression model.
  - **Feature Transformation:** Transforming data for better model performance, such as normalizing numerical data.
  - **Feature Creation:** Refers to the creation of new features from existing data to assist with better predictions.
    - Examples of feature creation include one-hot-encoding, binning, splitting, and calculated features.
  - **Feature Extraction:** Extracting useful information from raw data, such as deriving age from date of birth.
- Particularly meaningful for **Supervised Learning**.
- **Example**
  - **Before** Feature Engineering.
    | Customer_ID | Name | BirthDate | Purchase_Amount |
    |-------------|-------|------------|-----------------|
    | 1 | Alice | 15-05-1993 | $200 |
    | 2 | Bob | 22-08-1978 | $300 |
  - **After** Feature Engineering.
    | Customer_ID | Name | BirthDate | Purchase_Amount |
    |-------------|-------|-----------|-----------------|
    | 1 | Alice | 30 | $200 |
    | 2 | Bob | 45 | $300 |

#### 15.1.4.1. On Structured Data

- **Structured Data (Tabular Data)**
  - **Example:** Predicting house prices based on features like size, location, and number of rooms.
- **Feature Engineering Tasks**
  - **Feature Creation:** Deriving new features like "price per square foot".
  - **Feature Selection:** Identifying and retaining important features such as location or number of bedrooms.
  - **Feature Transformation:** Normalizing features to ensure they are on a similar scale, which helps algorithms like gradient descent converge faster.

#### 15.1.4.2. On Unstructured Data

- **Unstructured Data (Text, Images)**
  - **Example:** Sentiment analysis of customer reviews
- **Feature Engineering Tasks**
  - **Text Data:** Converting text into numerical features using techniques like TF-IDF or word embeddings.
  - **Image Data:** Extracting features such as edges or textures using techniques like convolutional neural networks (CNNs).

## 15.2. Unsupervised Learning

- The goal is to discover inherent patterns, structures, or relationships within the input data.
- The machine must uncover and create the groups itself, but humans still put labels on the output groups.
- Common techniques include Clustering, Association Rule Learning, and Anomaly Detection.
- **Clustering use cases:** Customer segmentation, targeted marketing, recommender systems.
- **Feature Engineering** can help improve the quality of the training.
- **Dimensionality Reduction** is an **unsupervised learning** technique that reduces the number of features (dimensions) in a dataset while preserving the most important information.

### 15.2.1. Clustering Technique

- Used to group similar data points together into clusters based on their features.
- **Example:** Customer Segmentation
  - **Scenario:** E-commerce company wants to segment its customers to understand different purchasing behaviors
  - **Data:** A dataset containing customer purchase history (e.g., purchase frequency, average order value).
  - **Goal:** Identify distinct groups of customers based on their purchasing behavior.
  - **Technique:** K-means Clustering.
- **Outcome:** The company can target each segment with tailored marketing strategies.

### 15.2.2. Association Rule Learning Technique

- **Example:** Market Basket Analysis.
  - **Scenario:** Supermarket wants to understand which products are frequently bought together.
  - **Data:** Transaction records from customer purchases.
  - **Goal:** Identify associations between products to optimize product placement and promotions.
  - **Technique:** Apriori algorithm.
- **Outcome:** The supermarket can place associated products together to boost sales.

### 15.2.3. Anomaly Detection Technique

- **Example:** Fraud Detection.
  - **Scenario:** Detect fraudulent credit card transactions.
  - **Data:** Transaction data, including amount, location, and time.
  - **Goal:** Identify transactions that deviate significantly from typical behavior.
  - **Technique:** Isolation Forest.
- **Outcome:** The system flags potentially fraudulent transactions for further investigation.

### 15.2.4. Dimensionality Reduction

- **Example**
  - **An image contains**
    - Person.
    - Sky.
    - Trees.
    - Buildings.
    - Cars.
- **Outcome:** Dimensionality reduction removes or minimizes unnecessary background features, allowing the model to focus on the **person**.

## 15.3. Semi-supervised Learning

- Use a small amount of labeled data and a large amount of unlabeled data to train systems.
  - After that, the partially trained algorithm itself labels the unlabeled data.
  - This is called pseudo-labeling.
  - The model is then re-trained on the resulting data mix without being explicitly programmed.

## 15.4. Self-Supervised Learning

- Self-supervised learning is a machine learning approach that applies unsupervised learning methods to tasks usually requiring supervised learning.
- Instead of using labeled datasets for guidance, self-supervised models create implicit labels from unstructured data.
- **Phases**
  1. Have a model generate pseudo labels for its own data without having humans label any data first.
  2. Then, using the pseudo labels, solve problems traditionally solved by Supervised Learning.
     - Widely used in NLP (to create the BERT and GPT models for example) and in image recognition tasks.

### 15.4.1. Intuitive example

- Create "pre-text tasks" to have the model solve simple tasks and learn patterns in the dataset.
- Pretext tasks are not "useful" as such, but will teach our model to create a "representation" of our dataset.
  - Predict any part of the input from any other part.
  - Predict the future from the past.
  - Predict the masked from the visible.
  - Predict any occluded part from all available parts.
- After solving the pre-text tasks, we have a model trained that can solve our end goal: "downstream tasks".

## 15.5. Transfer Learning

- **Transfer Learning** is the process of adapting a **pre-trained model** to perform a **new but related task** by training it on a new dataset.
- Reuses knowledge from a previously trained model for a related task.
- Improves performance with less training data and fewer computational resources.
- Adapts existing models using new data from similar domains.
- Ideal when multiple models can benefit from shared knowledge.

# 16. What is Reinforcement Learning (RL)?

- A type of Machine Learning where an agent learns to make decisions by performing actions in an environment to maximize cumulative rewards
- **Key Concepts**
  - **Agent:** The learner or decision-maker.
  - **Environment:** The external system the agent interacts with.
  - **Action:** The choices made by the agent.
  - **Reward:** The feedback from the environment based on the agent's actions.
  - **State:** The current situation of the environment.
  - **Policy:** The strategy the agent uses to determine.

## 16.1. How Does Reinforcement Learning Work?

- **Learning Process**
  - The Agent observes the current State of the Environment.
  - It selects an Action based on its Policy.
  - The environment transitions to a new State and provides a Reward.
  - The Agent updates its Policy to improve future decisions.
- **Goal:** Maximize cumulative reward over time.
  ![How Does Reinforcement Learning Work?](/Images/Machine%20Learning/HowDoesReinforcementLearningWork.png)

## 16.2. Reinforcement Learning in Action

- **Scenario:** Training a robot to navigate a maze.
- **Steps:** Robot (Agent) observes its position (State).
  - Chooses a direction to move (Action).
  - Receives a reward (-1 for taking a step, -10 for hitting a wall, +100 for going to the exit).
  - Updates its Policy based on the Reward and new position.
- **Outcome:** The robot learns to navigate the maze efficiently over time.

## 16.3. Applications of Reinforcement Learning

- Gaming - teaching AI to play complex games (e.g., Chess, Go).
- Robotics - navigating and manipulating objects in dynamic environments.
- Finance - portfolio management and trading strategies.
- Healthcare - optimizing treatment plans.
- Autonomous Vehicles - path planning and decision-making.

# 17. What is RLHF?

- RLHF = Reinforcement Learning from Human Feedback.
- Use human feedback to help ML models to self-learn more efficiently.
- In Reinforcement Learning there's a reward function.
- RLHF incorporates human feedback in the reward function, to be more aligned with human goals, wants and needs.
  - First, the model's responses are compared to human's responses.
  - Then, a human assess the quality of the model's responses.
- RLHF is used throughout GenAI applications including LLM Models.
- RLHF significantly enhances the model performance.
- **Example:** Grading text translations from "technically correct" to "human".

## 17.1. How does RLHF work?

- **Example:** Internal company knowledge chatbot
- **Data collection**
  - Set of human-generated prompts and responses are created.
  - "Where is the location of the HR department in Boston?"
- **Supervised fine-tuning of a language model**
  - Fine-tune an existing model with internal knowledge.
  - Then the model creates responses for the human-generated prompts.
  - Responses are mathematically compared to human-generated answers.
- **Build a separate reward model**
  - Humans can indicate which response they prefer from the same prompt.
  - The reward model can now estimate how a human would prefer a prompt response.
- **Optimize the language model with the reward-based model**
  - Use the reward model as a reward function for RL.
  - This part can be fully automated.

## 17.2. RLHF Process

- https://aws.amazon.com/what-is/reinforcement-learning-from-human-feedback/

# 18. Model Fit

- In case your model has poor performance, you need to look at its fit.
- **Overfitting**
  - Performs well on the training data.
  - Doesn't perform well on evaluation data.
- **Underfitting**
  - Model performs poorly on training data.
  - Could be a problem of having a model too simple or poor data features.
- **Balanced**
  - Neither overfitting or underfitting.
- **Summary**
  ![Overfitting vs Underfitting vs Balanced](/Images/Machine%20Learning/OverfittingUnderfittingBalanced.png)
  - [Font](https://www.geeksforgeeks.org/machine-learning/underfitting-and-overfitting-in-machine-learning/)

# 19. Bias and Variance

- **Bias**
  - Difference or error between predicted and actual value.
  - Occurs due to the wrong choice in the ML process.
- **High Bias**
  - The model doesn't closely match the training data.
  - **Example:** Linear regression function on a non-linear dataset.
  - Considered as underfitting.
- **Reducing the Bias**
  - Use a more complex model.
  - Increase the number of features.
- **Variance**
  - How much the performance of a model changes if trained on a different dataset which has a similar distribution.
- **High Variance**
  - Model is very sensitive to changes in the training data.
  - This is the case when overfitting: performs well on training data, but poorly on unseen test data.
- **Reducing the Variance**
  - Feature selection (less, more important features).
  - Split into training and test data sets multiple times.

# 20. Model Evaluation Metrics

## 20.1. Binary Classification

TODO DIAGRAM

## 20.2. Confusion Matrix

![Confusion Matrix](/Images/Machine%20Learning/ConfusionMatrix_1.png)

- Confusion Matrixes be multi-dimension too.
- Best way to evaluate the **performance** model that does **classifications**.
- **Metrics**
  - **Precision:** Best when false positives are costly.
  - **Recall:** Best when false negatives are costly.
  - **F1 Score:** Best when you want a balance between precision and recall, especially in imbalanced datasets.
  - **Accuracy:** Best for balanced datasets.
    ![Confusion Matrix](/Images/Machine%20Learning/ConfusionMatrix_2.png)

## 20.3. AUC-ROC

- Area under the curve-receiver operator curve.
- Value from 0 to 1 (perfect model).
- Uses sensitivity (true positive rate) and "1-specificity" (false positive rate).
- AUC-ROC shows what the curve for true positive compared to false positive looks like at various thresholds, with multiple confusion matrixes.
- You compare them to one another to find out the threshold you need for your business use case.

## 20.4. Regressions Metrics

- MAE, MAPE, RMSE, R² (R Squared) are used for evaluating models that predict a continuous value (i.e., regressions).
- **Example:** Imagine you're trying to predict how well students do on a test based on how many hours they study.
- MAE, MAPE, RMSE - measure the error: how "accurate" the model is.
  - If RMSE is 5, this means that, on average, your model's prediction of a student's score is about 5 points off from their actual score
- R² (R Squared) - measures the variance.
  - If R² is 0.8, this means that 80% of the changes in test scores can be explained by how much students studied, and the remaining 20% is due to other factors like natural ability or luck.

# 21. Inferencing

- Inferencing is when a model is making prediction on new data.
- **Real Time**
  - Computers have to make decisions quickly as data arrives.
  - Speed is preferred over perfect accuracy.
  - **Example:** Chatbots.
- **Batch**
  - Large amount of data that is analyzed all at once.
  - Often used for data analysis.
  - Speed of the results is usually not a concern, and accuracy is.

TODO DIAGRAM

## 21.1. Inferencing at the Edge

- Edge devices are usually devices with less computing power that are close to where the data is generated, in places where internet connections can be limited.
- **Small Language Model (SLM)** on the edge device.
  - Very low latency.
  - Low compute footprint.
  - Offline capability, local inference.
    ![Small Language Model (SLM)](/Images/Machine%20Learning/InferencingEdgeSLM.png)
- **Large Language Model (LLM)** on a remote server.
  - More powerful model.
  - Higher latency.
  - Must be online to be accessed.
    ![Large Language Model (LLM)](/Images/Machine%20Learning/InferencingEdgeLLM.png)

# 22. Phases of Machine Learning Project

![Phases of Machine Learning Project](/Images/Machine%20Learning/PhasesMachineLearningProject.png)

## 22.1. Phase details

- **Define business goals**
  - Stakeholders define the value, budget and success criteria.
  - Defining KPI (Key Performance Indicators) is critical.
- **ML problem framing**
  - Convert the business problem and into a machine learning problem.
  - Determine if ML is appropriate.
  - Data scientist, data engineers and ML architects and subject matter experts (SME) collaborate.
- **Data processing**
  - Convert the data into a usable format.
  - Data collection and integration (make it centrally accessible).
  - Data preprocessing and data visualization (understandable format).
  - **Feature engineering:** Create, transform and extract variables from data.
- **Model development**
  - Model training, tuning, and evaluation.
  - Iterative process.
  - Additional feature engineering and tune model hyperparameters.

## 22.2. EDA - Exploratory Data Analysis

- Visualize the data with graphs.
- **Correlation Matrix**
  - Look at correlations between variables (how "linked" they are).
  - Helps us decide which features can be important in your model.

## 22.3. Phases of Machine Learning Project

- **Retrain**
  - Look at data and features to improve the model.
  - Adjust the model training hyperparameters.
- **Deployment**
  - If results are good, the model is deployed and ready to make inferences.
  - Select a deployment model (real-time, serverless, asynchronous, batch, on-premises...).
- **Monitoring**
  - Deploy a system to check the desired level of performance.
  - Early detection and mitigation.
  - Debug issues and understand the model's behavior.
- **Iterations**
  - Model is continuously improved and refined as new data become available.
  - Requirements may change.
  - Iteration is important to keep the model accurate and relevant over time.

# 23. Hyperparameter Tuning

- **Hyperparameter**
  - Settings that define the model structure and learning algorithm and process.
  - **Set before training begins.**
  - **Examples:** Learning rate, batch size, number of epochs, and regularization.
- **Hyperparameter tuning**
  - Finding the best hyperparameters values to optimize the model performance.
  - Improves model accuracy, reduces overfitting, and enhances generalization.
- **How to do it?**
  - Grid search, random search.
  - Using services such as SageMaker Automatic Model Tuning (AMT).

## 23.1. Important Hyperparameters

- **Learning rate**
  - How large or small the steps are when updating the model's weights during training.
  - High learning rate can lead to faster convergence but risks overshooting the optimal solution, while a low learning rate may result in more precise but slower convergence.
- **Batch size**
  - Number of training examples used to update the model weights in one iteration.
  - Smaller batches can lead to more stable learning but require more time to compute, while larger batches are faster but may lead to less stable updates.
- **Number of Epochs**
  - Refers to how many times the model will iterate over the entire training dataset.
  - Too few epochs can lead to underfitting, while too many may cause overfitting.
- **Regularization**
  - Adjusting the balance between simple and complex model.
  - Increase regularization to reduce overfitting.

# 24. Model Parameters

- **Definition**
  - Internal values learned during training that determine **how the model makes predictions**.
  - Model parameters are values that define a model and its behavior in interpreting input and generating responses.
- **Examples**
  - Weights
  - Biases

# 25. Data Augmentation

- Generates new training data from existing data.
- Increases representation of underrepresented groups.
- Reduces dataset imbalance and model bias.
- Improves fairness and generalization.

# 26. What to do if overfitting?

- Overfitting is when the model gives good predictions for training data but not for the new data.
- **It occurs due to**
  - Training data size is too small and does not represent all possible input values.
  - The model trains too long on a single sample set of data.
  - Model complexity is high and learns from the "noise" within the training data.
- **How can you prevent overfitting?**
  - Increase the training data size.
  - Early stopping the training of the model.
  - Data augmentation (to increase diversity in the dataset).
  - **Adjust hyperparameters** (but you can't "add" them).
  - Ensembling (combine multiple models to get accurate results).

# 27. When is Machine Learning NOT appropriate?

- Imagine a well-framed problem like this one:
  - A deck contains five red cards, three blue cards, and two yellow cards. What is the probability of drawing a blue card?
- For deterministic problems (the solution can be computed), it is better to write computer code that is adapted to the problem.
- If we use Supervised Learning, Unsupervised Learning or Reinforcement Learning, we may have an "approximation" of the result.
- Even though nowadays LLMs have reasoning capabilities, they are not perfect and therefore a "worse" solution.

# 28. Model evaluation vs Model inference

- Model evaluation is the process of evaluating and comparing model outputs to determine the model that is best suited for a use case.
- Model inference is the process of a model generating an output (response) from a given input (prompt).

# 29. Rule-Based System

- A **Rule-Based System** makes decisions by following **predefined IF–THEN rules** created by humans.
- It **does not learn** from data.
- **Rule-Based System vs. Generative AI**
  | Rule-Based System | Machine Learning |
  | ---------------------- | ----------------------------- |
  | Human writes the rules | Model learns from data |
  | No training | Requires training |
  | Fixed behavior | Improves with more data |
  | Easy to explain | May be difficult to explain |
  | Best for known logic | Best for discovering patterns |

# 30. Deterministic vs. Probabilistic Models

## 30.1. Deterministic Models

- Always produce the **same output** for the same input.
- Predictable and consistent.
- No randomness in predictions.
- **Example**
  - Decision Trees

## 30.2. Probabilistic Models

- Predict a **probability or distribution** of possible outcomes.
- Incorporate uncertainty into predictions.
- Outputs may vary based on probabilities.
- **Example**
  - Bayesian Networks

## 30.3. Hybrid Models

- Combine deterministic and probabilistic behavior.
- Often produce probabilities that are converted into deterministic predictions.
- **Examples**
  - Neural Networks
  - Random Forests

# 31. CNN vs. RNN

| CNN (Convolutional Neural Network)                | RNN (Recurrent Neural Network)           |
| ------------------------------------------------- | ---------------------------------------- |
| Processes **spatial data**                        | Processes **sequential data**            |
| Best for **images**                               | Best for **time-series and sequences**   |
| Learns spatial features (shapes, edges, textures) | Learns temporal dependencies and context |
| Input order is not important                      | Input order is important                 |

## 31.1. CNN (Convolutional Neural Network)

- **Purpose**
  - Analyze grid-like data, especially images.
- **Common Use Cases**
  - Image classification.
  - Object detection.
  - Face recognition.
  - Medical image analysis.
- **Example**
  - Analyze a single photo to determine whether it contains a cat or a dog.

## 31.2. RNN (Recurrent Neural Network)

- **Purpose**
  - Analyze sequential data where previous inputs influence future predictions.
- **Common Use Cases**
  - Video analysis.
  - Speech recognition.
  - Language translation.
  - Time-series forecasting.
- **Example**
  - Analyze a video by processing each frame in sequence to recognize an action.
