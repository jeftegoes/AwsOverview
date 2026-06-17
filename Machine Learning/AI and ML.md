# IA and ML <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. What is Artificial Intelligence (AI)?](#1-what-is-artificial-intelligence-ai)
  - [1.1. Use Cases](#11-use-cases)
- [2. AI Components](#2-ai-components)
- [3. What is Machine Learning (ML)?](#3-what-is-machine-learning-ml)
  - [3.1. AI != ML](#31-ai--ml)
- [4. What is Deep Learning (DL)?](#4-what-is-deep-learning-dl)
- [5. Neural Networks - How do they work?](#5-neural-networks---how-do-they-work)
- [6. What is Generative AI (Gen-AI)?](#6-what-is-generative-ai-gen-ai)
- [7. What is the Transformer Model? (LLM)](#7-what-is-the-transformer-model-llm)
- [8. Diffusion Models (ex: Stable Diffusion)](#8-diffusion-models-ex-stable-diffusion)
- [9. Multi-modal Models (ex: GPT-4o)](#9-multi-modal-models-ex-gpt-4o)
- [10. Humans are a mix of AI](#10-humans-are-a-mix-of-ai)
- [11. ML Terms](#11-ml-terms)
- [12. Training Data](#12-training-data)
  - [12.1. Labeled vs. Unlabeled Data](#121-labeled-vs-unlabeled-data)
  - [12.2. Structured Data](#122-structured-data)
  - [12.3. Unstructured Data](#123-unstructured-data)
- [13. ML Algorithms](#13-ml-algorithms)
  - [13.1. Supervised Learning](#131-supervised-learning)
    - [13.1.1. Regression](#1311-regression)
    - [13.1.2. Classification](#1312-classification)
    - [13.1.3. Training vs. Validation vs. Test Set](#1313-training-vs-validation-vs-test-set)
    - [13.1.4. Feature Engineering](#1314-feature-engineering)
      - [13.1.4.1. On Structured Data](#13141-on-structured-data)
      - [13.1.4.2. On Unstructured Data](#13142-on-unstructured-data)
  - [13.2. Unsupervised Learning](#132-unsupervised-learning)
    - [13.2.1. Clustering Technique](#1321-clustering-technique)
    - [13.2.2. Association Rule Learning Technique](#1322-association-rule-learning-technique)
    - [13.2.3. Anomaly Detection Technique](#1323-anomaly-detection-technique)
  - [13.3. Semi-supervised Learning](#133-semi-supervised-learning)
  - [13.4. Self-Supervised Learning](#134-self-supervised-learning)
    - [13.4.1. Intuitive example](#1341-intuitive-example)
- [14. What is Reinforcement Learning (RL)?](#14-what-is-reinforcement-learning-rl)
  - [14.1. How Does Reinforcement Learning Work?](#141-how-does-reinforcement-learning-work)
  - [14.2. Reinforcement Learning in Action](#142-reinforcement-learning-in-action)
  - [14.3. Applications of Reinforcement Learning](#143-applications-of-reinforcement-learning)
- [15. What is RLHF?](#15-what-is-rlhf)
  - [15.1. How does RLHF work?](#151-how-does-rlhf-work)
  - [15.2. RLHF Process](#152-rlhf-process)
- [16. Model Fit](#16-model-fit)
- [17. Bias and Variance](#17-bias-and-variance)
- [18. Model Evaluation Metrics](#18-model-evaluation-metrics)
  - [18.1. Binary Classification](#181-binary-classification)
  - [18.2. Confusion Matrix](#182-confusion-matrix)
  - [18.3. AUC-ROC](#183-auc-roc)
  - [18.4. Regressions Metrics](#184-regressions-metrics)
- [19. Inferencing](#19-inferencing)
  - [19.1. Inferencing at the Edge](#191-inferencing-at-the-edge)

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

# 6. What is Generative AI (Gen-AI)?

- Subset of Deep Learning.
- Multi-purpose foundation models backed by neural networks.
- They can be fine-tuned if necessary to better fit our use-cases.

# 7. What is the Transformer Model? (LLM)

- Able to process a sentence as a whole instead of word by word.
- Faster and more efficient text processing (less training time).
- It gives relative importance to specific words in a sentence (more coherent sentences).
- Transformer-based LLMs.
  - Powerful models that can understand and generate human-like text.
  - Trained on vast amounts of text data from the internet, books, and other sources, and learn patterns and relationships between words and phrases.
  - **Example:** Google BERT, OpenAI ChatGPT.
  - (ChatGPT = Chat Generative Pretrained Transformer).

# 8. Diffusion Models (ex: Stable Diffusion)

TODO DIAGRAM

# 9. Multi-modal Models (ex: GPT-4o)

- Does NOT rely on a single type of input (text, or images, or audio only).
- Does NOT create a single type of output.
- Example: a multi-modal can take a mix of audio, image and text and output a mix of video, text for example.

# 10. Humans are a mix of AI

- Sometimes we know "if this happens, then do that" (AI).
- Sometimes we've seen a lot of similar things before, and we classify them (Machine Learning).
- Sometimes we haven't seen something before, but we have "learned" a lot of similar concepts, so we can make a decision (Deep Learning).
- Sometimes, we get creative, and based on what we've learned, we can generate content: Gen AI.

# 11. ML Terms

- GPT (Generative Pre-trained Transformer) - generate human text or computer code based on input prompts.
- BERT (Bidirectional Encoder Representations from Transformers) - similar intent to GPT, but reads the text in two directions.
- RNN (Recurrent Neural Network) - meant for sequential data such as time-series or text, useful in speech recognition, time-series prediction.
- ResNet (Residual Network) - Deep Convolutional Neural Network (CNN) used for image recognition tasks, object detection, facial recognition.
- SVM (Support Vector Machine) - ML algorithm for classification and regression.
- WaveNet - model to generate raw audio waveform, used in Speech Synthesis.
- GAN (Generative Adversarial Network) - models used to generate synthetic data such as images, videos or sounds that resemble the training data. Helpful for data augmentation.
- XGBoost (Extreme Gradient Boosting) - an implementation of gradient boosting.

# 12. Training Data

- To train our model we must have good data.
- Garbage in => Garbage out.
- Most critical stage to build a good model.
- Several options to model our data, which will impact the types of algorithms we can use to train our models.
- Labeled vs. Unlabeled Data.
- Structured vs. Unstructured Data.

## 12.1. Labeled vs. Unlabeled Data

- **Labeled Data**
  - Data includes both input features and corresponding output labels
  - **Example:** Dataset with images of animals where each image is labeled with the corresponding animal type (e.g., cat, dog)
  - **Use case:** **Supervised Learning**, where the model is trained to map inputs to known outputs
- **Unlabeled Data**
  - Data includes only input features without any output labels.
  - **Example:** A collection of images without any associated labels.
  - **Use case:** **Unsupervised Learning**, where the model tries to find patterns or structures in the data.

## 12.2. Structured Data

- Data is organized in a structured format, often in rows and columns (like Excel).
- **Tabular Data**
  - Data is arranged in a table with rows representing records and columns representing features.
  - **Example**
  - TODO: example
- **Time Series Data**
  - Data points collected or recorded at successive points in time.
  - **Example:** Stock prices recorded daily over a year.
    TODO: example

## 12.3. Unstructured Data

- Data that doesn't follow a specific structure and is often text-heavy or multimedia content.
- **Text Data**
  - Unstructured text such as articles, social media posts, or customer reviews.
  - **Example:** A collection of product reviews from an e-commerce site.
- **Image Data**
  - Data in the form of images, which can vary widely in format and content.
  - Example: Images used for object recognition tasks.

# 13. ML Algorithms

## 13.1. Supervised Learning

- Learn a mapping function that can predict the output for new unseen input data.
- **Needs labeled data:** Very powerful, but difficult to perform on millions of datapoints.

### 13.1.1. Regression

- Used to predict a numeric value based on input data.
- The output variable is continuous, meaning it can take any value within a range.
- Use cases: Used when the goal is to predict a quantity or a real value.
- **Examples**
  - Predicting House Prices - based on features like size, location, and number of bedrooms.
  - Stock Price Prediction - predicting the future price of a stock based on historical data and other features.
  - Weather Forecasting - predicting temperatures based on historical weather data.

### 13.1.2. Classification

- Used to predict the categorical label of input data.
- The output variable is discrete, which means it falls into a specific category or class.
- **Use cases:** Scenarios where decisions or predictions need to be made between distinct categories (fraud, image classification, customer retention, diagnostics).
- **Examples**
  - **Binary Classification:** Classify emails as "spam" or "not spam".
  - **Multiclass Classification:** Classify animals in a zoo as "mammal," "bird," "reptile".
  - **Multi-label Classification:** Assign multiple labels to a movie, like "action" and "comedy.
- **Key algorithm:** K-nearest neighbors (k-NN) model.

### 13.1.3. Training vs. Validation vs. Test Set

- **Training Set**
  - Used to train the model.
  - Percentage: Typically, 60-80% of the dataset.
  - Example: 800 labeled images from a dataset of 1000 images.
- **Validation Set**
  - Used to tune model parameters and validate performance.
  - Percentage: Typically, 10-20% of the dataset.
  - Example: 100 labeled images for hyperparameter tuning (tune the settings of the algorithm to make it more efficient).
- **Test Set**
  - Used to evaluate the final model performance.
  - Percentage: Typically, 10-20% of the dataset.
  - Example: 100 labeled images to test the model's accuracy.

### 13.1.4. Feature Engineering

- The process of using domain knowledge to select and transform raw data into meaningful features.
- Helps enhancing the performance of machine learning models.
- **Techniques**
  - Feature Extraction - extracting useful information from raw data, such as deriving age from date of birth.
  - Feature Selection - selecting a subset of relevant features, like choosing important predictors in a regression model.
  - Feature Transformation - transforming data for better model performance, such as normalizing numerical data.
- Particularly meaningful for Supervised Learning.

#### 13.1.4.1. On Structured Data

- Structured Data (Tabular Data).
- **Example:** Predicting house prices based on features like size, location, and number of rooms.
- **Feature Engineering Tasks**
  - Feature Creation - deriving new features like "price per square foot".
  - Feature Selection - identifying and retaining important features such as location or number of bedrooms.
  - Feature Transformation - normalizing features to ensure they are on a similar scale, which helps algorithms like gradient descent converge faster.

#### 13.1.4.2. On Unstructured Data

- Unstructured Data (Text, Images).
- **Example:** Sentiment analysis of customer reviews
- **Feature Engineering Tasks**
  - Text Data - converting text into numerical features using techniques like TF-IDF or word embeddings.
  - Image Data - extracting features such as edges or textures using techniques like convolutional neural networks (CNNs).

## 13.2. Unsupervised Learning

- The goal is to discover inherent patterns, structures, or relationships within the input data.
- The machine must uncover and create the groups itself, but humans still put labels on the output groups.
- Common techniques include Clustering, Association Rule Learning, and Anomaly Detection.
- **Clustering use cases:** Customer segmentation, targeted marketing, recommender systems.
- **Feature Engineering** can help improve the quality of the training.

### 13.2.1. Clustering Technique

- Used to group similar data points together into clusters based on their features.
- **Example:** Customer Segmentation
  - **Scenario:** E-commerce company wants to segment its customers to understand different purchasing behaviors
  - **Data:** A dataset containing customer purchase history (e.g., purchase frequency, average order value).
  - **Goal:** Identify distinct groups of customers based on their purchasing behavior.
  - **Technique:** K-means Clustering.
- **Outcome:** The company can target each segment with tailored marketing strategies.

### 13.2.2. Association Rule Learning Technique

- **Example:** Market Basket Analysis
  - **Scenario:** Supermarket wants to understand which products are frequently bought together.
  - **Data:** Transaction records from customer purchases.
  - **Goal:** Identify associations between products to optimize product placement and promotions.
  - **Technique:** Apriori algorithm.
- **Outcome: ** The supermarket can place associated products together to boost sales.

### 13.2.3. Anomaly Detection Technique

- **Example:** Fraud Detection.
  - **Scenario:** Detect fraudulent credit card transactions.
  - **Data:** Transaction data, including amount, location, and time.
  - **Goal:** Identify transactions that deviate significantly from typical behavior.
  - **Technique:** Isolation Forest.
- **Outcome:** The system flags potentially fraudulent transactions for further investigation.

## 13.3. Semi-supervised Learning

- Use a small amount of labeled data and a large amount of unlabeled data to train systems.
- After that, the partially trained algorithm itself labels the unlabeled data.
- This is called pseudo-labeling.
- The model is then re-trained on the resulting data mix without being explicitly programmed.

## 13.4. Self-Supervised Learning

- Have a model generate pseudo labels for its own data without having humans label any data first.
- Then, using the pseudo labels, solve problems traditionally solved by Supervised Learning.
- Widely used in NLP (to create the BERT and GPT models for example) and in image recognition tasks.

### 13.4.1. Intuitive example

- Create "pre-text tasks" to have the model solve simple tasks and learn patterns in the dataset.
- Pretext tasks are not "useful" as such, but will teach our model to create a "representation" of our dataset.
  - Predict any part of the input from any other part.
  - Predict the future from the past.
  - Predict the masked from the visible.
  - Predict any occluded part from all available parts.
- After solving the pre-text tasks, we have a model trained that can solve our end goal: "downstream tasks".

# 14. What is Reinforcement Learning (RL)?

- A type of Machine Learning where an agent learns to make decisions by performing actions in an environment to maximize cumulative rewards
- **Key Concepts**
  - **Agent** - The learner or decision-maker.
  - **Environment** - The external system the agent interacts with.
  - **Action** - The choices made by the agent.
  - **Reward** - The feedback from the environment based on the agent's actions.
  - **State** - The current situation of the environment.
  - **Policy** - The strategy the agent uses to determine.

## 14.1. How Does Reinforcement Learning Work?

- **Learning Process**
  - The Agent observes the current State of the Environment.
  - It selects an Action based on its Policy.
  - The environment transitions to a new State and provides a Reward.
  - The Agent updates its Policy to improve future decisions.
- **Goal:** Maximize cumulative reward over time.

## 14.2. Reinforcement Learning in Action

- **Scenario:** Training a robot to navigate a maze.
- **Steps:** Robot (Agent) observes its position (State).
  - Chooses a direction to move (Action).
  - Receives a reward (-1 for taking a step, -10 for hitting a wall, +100 for going to the exit).
  - Updates its Policy based on the Reward and new position.
- **Outcome:** The robot learns to navigate the maze efficiently over time.

## 14.3. Applications of Reinforcement Learning

- Gaming - teaching AI to play complex games (e.g., Chess, Go).
- Robotics - navigating and manipulating objects in dynamic environments.
- Finance - portfolio management and trading strategies.
- Healthcare - optimizing treatment plans.
- Autonomous Vehicles - path planning and decision-making.

# 15. What is RLHF?

- RLHF = Reinforcement Learning from Human Feedback.
- Use human feedback to help ML models to self-learn more efficiently.
- In Reinforcement Learning there's a reward function.
- RLHF incorporates human feedback in the reward function, to be more aligned with human goals, wants and needs.
  - First, the model's responses are compared to human's responses.
  - Then, a human assess the quality of the model's responses.
- RLHF is used throughout GenAI applications including LLM Models.
- RLHF significantly enhances the model performance.
- **Example:** Grading text translations from "technically correct" to "human".

## 15.1. How does RLHF work?

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

## 15.2. RLHF Process

- https://aws.amazon.com/what-is/reinforcement-learning-from-human-feedback/

# 16. Model Fit

- In case your model has poor performance, you need to look at its fit.
- **Overfitting**
  - Performs well on the training data.
  - Doesn't perform well on evaluation data.
- **Underfitting**
  - Model performs poorly on training data.
  - Could be a problem of having a model too simple or poor data features.
- **Balanced**
  - Neither overfitting or underfitting.

# 17. Bias and Variance

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

# 18. Model Evaluation Metrics

## 18.1. Binary Classification

TODO DIAGRAM

## 18.2. Confusion Matrix

TODO DIAGRAM

- Confusion Matrixes be multi-dimension too.
- Best way to evaluate the performance model that does classifications
- **Metrics**
  - **Precision** - Best when false positives are costly.
  - **Recall** - Best when false negatives are costly.
  - **F1 Score** - Best when you want a balance between precision and recall, especially in imbalanced datasets.
  - **Accuracy** - Best for balanced datasets.

## 18.3. AUC-ROC

- Area under the curve-receiver operator curve.
- Value from 0 to 1 (perfect model).
- Uses sensitivity (true positive rate) and "1-specificity" (false positive rate).
- AUC-ROC shows what the curve for true positive compared to false positive looks like at various thresholds, with multiple confusion matrixes.
- You compare them to one another to find out the threshold you need for your business use case.

## 18.4. Regressions Metrics

- MAE, MAPE, RMSE, R² (R Squared) are used for evaluating models that predict a continuous value (i.e., regressions).
- **Example:** Imagine you're trying to predict how well students do on a test based on how many hours they study.
- MAE, MAPE, RMSE - measure the error: how "accurate" the model is.
  - If RMSE is 5, this means that, on average, your model's prediction of a student's score is about 5 points off from their actual score
- R² (R Squared) - measures the variance.
  - If R² is 0.8, this means that 80% of the changes in test scores can be explained by how much students studied, and the remaining 20% is due to other factors like natural ability or luck.

# 19. Inferencing

- Inferencing is when a model is making prediction on new data.
- **Real Time**
  - Computers have to make decisions quickly as data arrives.
  - Speed is preferred over perfect accuracy.
  - Example: chatbots.
- **Batch**
  - Large amount of data that is analyzed all at once.
  - Often used for data analysis.
  - Speed of the results is usually not a concern, and accuracy is.

## 19.1. Inferencing at the Edge

- Edge devices are usually devices with less computing power that are close to where the data is generated, in places where internet connections can be limited.
- **Small Language Model (SLM)** on the edge device.
  - Very low latency.
  - Low compute footprint.
  - Offline capability, local inference.
- **Large Language Model (LLM)** on a remote server.
  - More powerful model.
  - Higher latency.
  - Must be online to be accessed.
