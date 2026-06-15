# Responsible AI, Security, Compliance and Governance for AI Solutions <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Responsible AI \& Security](#1-responsible-ai--security)
- [2. Governance \& Compliance](#2-governance--compliance)
- [3. Core dimensions of responsible AI](#3-core-dimensions-of-responsible-ai)
- [4. Responsible AI - AWS Services](#4-responsible-ai---aws-services)
- [5. AWS AI Service Cards](#5-aws-ai-service-cards)
- [6. Interpretability Trade-Offs](#6-interpretability-trade-offs)
  - [6.1. High Interpretability - Decision Trees](#61-high-interpretability---decision-trees)
  - [6.2. Partial Dependence Plots (PDP)](#62-partial-dependence-plots-pdp)
- [7. Human-Centered Design (HCD) for Explainable AI](#7-human-centered-design-hcd-for-explainable-ai)
- [8. Gen. AI Capabilities \& Challenges](#8-gen-ai-capabilities--challenges)

# 1. Responsible AI & Security

- **Responsible AI**
  - Making sure AI systems are transparent and trustworthy.
  - Mitigating potential risk and negative outcomes.
  - Throughout the AI lifecycle: design, development, deployment, monitoring, evaluation.
- **Security**
  - Ensure that confidentiality, integrity, and availability are maintained.
  - On organizational data and information assets and infrastructure.

# 2. Governance & Compliance

- **Governance**
  - Ensure to add value and manage risk in the operation of business.
  - Clear policies, guidelines, and oversight mechanisms to ensure AI systems align with legal and regulatory requirements.
  - Improve trust.
- **Compliance**
  - Ensure adherence to regulations and guidelines.
  - Sensitive domains such as healthcare, finance, and legal applications.

# 3. Core dimensions of responsible AI

- **Fairness:** Promote inclusion and prevent discrimination.
- Explainability.
- **Privacy and security:** Individuals control when and if their data is used.
- Transparency.
- **Veracity and robustness:** Reliable even in unexpected situations.
- **Governance:** Define, implement and enforce responsible AI practices.
- **Safety:** Algorithms are safe and beneficial for individuals and society.
- **Controllability:** Ability to align to human values and intent.

# 4. Responsible AI - AWS Services

- **Amazon Bedrock:** Human or automatic model evaluation.
- **Guardrails for Amazon Bedrock**
  - Filter content, redact PII, enhanced safety and privacy...
  - Block undesirable topics.
  - Filter harmful content.
- **SageMaker Clarify**
  - FM evaluation on accuracy, robustness, toxicity.
  - Bias detection (ex: data skewed towards middle-aged people).
- **SageMaker Data Wrangler:** Fix bias by balancing dataset.
  - **Ex:** Augment the data (generate new instances of data for underrepresented groups).
- **SageMaker Model Monitor:** Quality analysis in production.
- **Amazon Augmented AI (A2I):** Human review of ML predictions.
- **Governance:** SageMaker Role Manager, Model Cards, Model Dashboard.

# 5. AWS AI Service Cards

- Form of responsible AI documentation.
- Help understand the service and its features.
- Find intended use cases and limitations.
- Responsible AI design choices.
- Deployment and performance optimization best practices https://aws.amazon.com/machine-learning/responsible-machine-learning/textract-analyzeid/

# 6. Interpretability Trade-Offs

- Interpretability
  - The degree to which a human can understand the cause of a decision.
  - Access into the system so that a human can interpret the model's output.
  - Answer "why and how".
- High transparency => High interpretability => Poor performance.
- **Explainability**
  - Understand the nature and behavior of the model.
  - Being able to look at inputs and outputs and explain without understanding exactly how the model came to the conclusion.
- Explainability can sometimes be enough

## 6.1. High Interpretability - Decision Trees

- Supervised Learning Algorithm used for **Classification** and **Regression** tasks.
- Splits data into branches based on feature values.
- Splitting can be simple rules such as "is the feature greater than 5?".
- Prone to overfitting if you have too many branches.
- Easy to interpret, clear visual representation.

## 6.2. Partial Dependence Plots (PDP)

- Show how a single feature can influence the predicted outcome, while holding other features constant.
- Particularly helpful when the model is "black box" (i.e., Neural Networks).
- Helps with interpretability and explainability.

# 7. Human-Centered Design (HCD) for Explainable AI

- Approach to design AI systems with priorities for humans' needs.
- **Design for amplified decision-making**
  - Minimize risk and errors in a stressful or high-pressure environment.
  - Design for clarity, simplicity, usability.
  - Design for reflexivity (reflect on decision-making process) and accountability.
- **Design for unbiased decision-making**
  - Decision process is free from bias.
  - Train decision-makers to recognize and mitigate biases.
- **Design for human and AI learning**
  - **Cognitive apprenticeship:** AI systems learn from human instructors and experts.
  - **Personalization:** Meet the specific needs and preference of a human learner.
  - **User-centered design:** Accessible to a wide range of users.

# 8. Gen. AI Capabilities & Challenges

- **Capabilities of Generative AI**
  - Adaptability.
  - Responsiveness.
  - Simplicity.
  - Creativity and exploration.
  - Data efficiency.
  - Personalization.
  - Scalability.
- **Challenges of Generative AI**
  - Regulatory violations.
  - Social risks.
  - Data security and privacy concerns.
  - Toxicity.
  - Hallucinations.
  - Interpretability.
  - Nondeterminism.
  - Plagiarism and cheating.
