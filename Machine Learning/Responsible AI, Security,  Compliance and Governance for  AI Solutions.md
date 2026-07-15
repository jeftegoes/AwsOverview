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
  - [8.1. Toxicit](#81-toxicit)
  - [8.2. Hallucinations](#82-hallucinations)
  - [8.3. Plagiarism and Cheating](#83-plagiarism-and-cheating)
  - [8.4. Prompt Misuses](#84-prompt-misuses)
    - [8.4.1. Poisoning](#841-poisoning)
    - [8.4.2. Hijacking and Prompt Injection](#842-hijacking-and-prompt-injection)
    - [8.4.3. Exposure](#843-exposure)
    - [8.4.4. Prompt Leaking](#844-prompt-leaking)
    - [8.4.5. Jailbreaking](#845-jailbreaking)
- [9. Regulated Workloads](#9-regulated-workloads)
- [10. AI Standard Compliance Challenges](#10-ai-standard-compliance-challenges)
- [11. AWS Compliance](#11-aws-compliance)
- [12. Model Cards](#12-model-cards)
- [13. Importance of Governance \& Compliance](#13-importance-of-governance--compliance)
- [14. Governance Framework](#14-governance-framework)
  - [14.1. AWS Tools for Governance](#141-aws-tools-for-governance)
  - [14.2. Governance Strategies](#142-governance-strategies)
  - [14.3. Data Governance Strategies](#143-data-governance-strategies)
  - [14.4. Data Management Concepts](#144-data-management-concepts)
  - [14.5. Data Lineage](#145-data-lineage)
- [15. Security and Privacy for AI Systems](#15-security-and-privacy-for-ai-systems)
  - [15.1. Security and Privacy for AI Systems](#151-security-and-privacy-for-ai-systems)
  - [15.2. Monitoring AI systems](#152-monitoring-ai-systems)
  - [15.3. AWS Shared Responsibility Model](#153-aws-shared-responsibility-model)
  - [15.4. Secure Data Engineering - Best Practices](#154-secure-data-engineering---best-practices)
  - [15.5. Generative AI - Security Scoping Matrix](#155-generative-ai---security-scoping-matrix)
- [16. MLOps](#16-mlops)

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

- **Interpretability**
  - The degree to which a human can understand the cause of a decision.
  - Access into the system so that a human can interpret the model's output.
  - Answer "why and how".
- High transparency => High interpretability => Poor performance.
- **Explainability**
  - Understand the nature and behavior of the model.
  - Being able to look at inputs and outputs and explain without understanding exactly how the model came to the conclusion.
- Explainability can sometimes be enough.
  ![Interpretability vs Explainability](/Images/Machine%20Learning/InterpretabilityExplainability.png)

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

## 8.1. Toxicit

- Generating content that is offensive, disturbing, or inappropriate.
- Defining what constitutes "toxicity" can be a challenge.
- Boundary between restricting toxic content and censorship.
- What about quotations of someone that can be considered toxic? Should they be included?
- **Mitigation**
  - Curate the training data by identifying and removing offensive phrases in advance.
  - Use guardrail models to detect and filter out unwanted content.

## 8.2. Hallucinations

- Assertions or claims that sound true, but are incorrect.
- This is due to the next-word probability sampling employed by LLM.
- This can lead to content that may not exist, even though the content may seem plausible.
- **Mitigation**
  - Educate users that content generated by the model must be checked.
  - Ensure verification of content with independent sources.
  - Mark generated content as unverified to alert users that verification is necessary.

## 8.3. Plagiarism and Cheating

- Worries that Gen AI can be used to write college essays, writing samples for job applications, and other forms of cheating or illicit copying.
- Debates on this topic are actively happening.
- Some are saying the new technologies should be accepted, and other say it should be banned.
- Difficulties in tracing the source of a specific output of an LLM.
- Rise of technologies to detect if text or images have been generated with AI.

## 8.4. Prompt Misuses

### 8.4.1. Poisoning

- Intentional introduction of malicious or biased data into the training dataset of a model.
- Leads to the model producing biased, offensive, or harmful outputs (intentionally or unintentionally).

### 8.4.2. Hijacking and Prompt Injection

- Influencing the outputs by embedding specific instructions within the prompts themselves
- Hijack the model's behavior and make it produce outputs that align with the attacker's intentions (e.g., generating misinformation or running malicious code).
- **Example**
  - A malicious actor could craft prompts for a text generation model that contain harmful, unethical, or biased content.

### 8.4.3. Exposure

- The risk of exposing sensitive or confidential information to a model during training or inference.
- The model can then reveal this sensitive data from their training corpus, leading to potential data leaks or privacy violations.

### 8.4.4. Prompt Leaking

- The unintentional disclosure or leakage of the prompts or inputs used within a model.
- It can expose protected data or other data used by the model, such as how the model works.

### 8.4.5. Jailbreaking

- AI models are typically trained with certain ethical and safety constraints in place to prevent misuse or harmful outputs (e.g., filtering out offensive content, restricting access to sensitive information...).
- Circumvent the constraints and safety measures implemented in a generative model to gain unauthorized access or functionality.

# 9. Regulated Workloads

- Some industries require extra level of Compliance:
  - Financial services.
  - Healthcare.
  - Aerospace.
- **Examples**
  - Reporting regularly to federal agencies.
  - **Regulated outcome:** Mortgage and credit applications.
- If we need to comply with regulatory frameworks (audit, archival, special security requirements...), then we have a regulated workload!

# 10. AI Standard Compliance Challenges

- **Complexity and Opacity:** Challenging to audit how systems make decisions
- **Dynamism and Adaptability:** AI systems change over time, not static
- **Emergent Capabilities:** Unintended capabilities a system may have
- **Unique Risks**
  - Algorithmic bias, privacy violations, misinformation...
    - **Algorithmic Bias:** If the data is biased (not representative), the model can perpetuate bias
    - **Human Bias:** The humans who create the AI system can also introduce bias
- **Algorithm accountability:** Algorithms should be transparent and explainable
  - Regulations in the EU "Artificial Intelligence Act" and US (several states and cities)
  - Promotes fairness, non-discrimination and human rights

# 11. AWS Compliance

- Over 140 security standards and compliance certifications.
- National Institute of Standards and Technology (NIST).
- European Union Agency for Cybersecurity (ENISA).
- International Organization for Standardization (ISO).
- AWS System and Organization Controls (SOC).
- Health Insurance Portability and Accountability Act (HIPAA).
- General Data Protection Regulation (GDPR).
- Payment Card Industry Data Security Standard (PCI DSS).

# 12. Model Cards

- Standardized format for documenting the key details about an ML model.
- In generative AI, can include source citations and data origin documentation.
- Details about the datasets used, their sources, licenses, and any known biases or quality issues in the training data.
- Intended use, risk rating of a model, training details and metrics.
- **SageMaker Model Cards:** Document your ML models in a centralized place.
- Helpful to support audit activities.
- AWS AI Service Cards are examples SageMaker Model card.
- **Model card JSON schema sample file**
  ```json
    "intended_uses": {
      "description": "Intended usage of model",
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "purpose_of_model": {
          "description": "Why the model was developed?",
          "type": "string",
          "maxLength": 2048
        },
        "intended_uses": {
          "description": "intended use cases",
          "type": "string",
          "maxLength": 2048
        },
        "factors_affecting_model_efficiency": {
          "type": "string",
          "maxLength": 2048
        },
        "risk_rating": {
          "description": "Risk rating for model card",
          "$ref": "#/definitions/risk_rating"
        },
        "explanations_for_risk_rating": {
          "type": "string",
          "maxLength": 2048
        }
      }
    }
  ```

# 13. Importance of Governance & Compliance

- Managing, optimizing, and scaling the organizational AI initiative.
- Governance is instrumental to build trust.
- Ensure responsible and trustworthy AI practices.
- **Mitigate risks:** Bias, privacy violations, unintended consequences...
- Establish clear policies, guidelines, and oversight mechanisms to ensure AI systems align with legal and regulatory requirements.
- Protect from potential legal and reputational risks.
- Foster public trust and confidence in the responsible deployment of AI.

# 14. Governance Framework

- **Example approach**
  - Establish an AI Governance Board or Committee - this team should include representatives from various departments, such as legal, compliance, data privacy, and Subject Matter Experts (SMEs) in AI development.
  - Define Roles and Responsibilities - outline the roles and responsibilities of the governance board (e.g., oversight, policy-making, risk assessment, and decision making processes).
  - Implement Policies and Procedures - develop comprehensive policies and procedures that address the entire AI lifecycle, from data management to model deployment and monitoring.

## 14.1. AWS Tools for Governance

- AWS Config
- AWS Artifact
- Amazon Inspector
- AWS CloudTrail
- AWS Audit Manager
- AWS Trusted Advisor

## 14.2. Governance Strategies

- **Policies:** Principles, guidelines, and responsible AI considerations.
  - Data management, model training, output validation, safety, and human oversight.
  - Intellectual property, bias mitigation, and privacy protection.
- **Review Cadence:** Combination of technical, legal, and responsible AI review.
  - **Clear timeline:** Monthly, quarterly, annually...
  - Include Subject Matter Experts (SMEs), legal and compliance teams and end-users.
- **Review Strategies**
  - Technical reviews on model performance, data quality, algorithm robustness.
  - Non-technical reviews on policies, responsible AI principles, regulatory requirements.
  - Testing and validation procedure for outputs before deploying a new model.
  - Clear decision-making frameworks to make decisions based on review results.
- **Transparency Standards**
  - Publishing information about the AI models, training data, key decisions made.
  - Documentation on limitations, capabilities and use cases of AI solutions.
  - Channels for end-users and stakeholders to provide feedback and raise concerns.
- **Team Training Requirements**
  - Train on relevant policies, guidelines, and best practices.
  - Training on bias mitigation and responsible AI practices.
  - Encourage cross-functional collaboration and knowledge-sharing.
  - Implement a training and certification program.

## 14.3. Data Governance Strategies

- **Responsible AI**
  - Responsible framework and guidelines (bias, fairness, transparency, accountability).
  - Monitor AI and Generative AI for potential bias, fairness issue, and unintended consequences.
  - Educate and train teams on responsible AI practices.
- **Governance Structure and Roles**
  - Establish a data governance council or committee.
  - Define clear roles and responsibilities for data stewards, data owners, and data custodians.
  - Provide training and support to AI & ML practitioners.
- **Data Sharing and Collaboration**
  - Data sharing agreements to share data securely within the company.
  - Data virtualization or federation to give access to data without compromising ownership.
  - Foster a culture of data-driven decision-making and collaborative data governance.

## 14.4. Data Management Concepts

- **Data Lifecycles:** Collection, processing, storage, consumption, archival.
- **Data Logging:** Tracking inputs, outputs, performance metrics, system events.
- **Data Residency:** Where the data is processed and stored (regulations, privacy requirements, proximity of compute and data).
- **Data Monitoring:** Data quality, identifying anomalies, data drift.
- **Data Analysis:** Statistical analysis, data visualization, exploration.
- **Data Retention:** Regulatory requirements, historical data for training, cost.

## 14.5. Data Lineage

- **Source Citation**
  - Attributing and acknowledging the sources of the data.
  - Datasets, databases, other sources.
  - Relevant licenses, terms of use, or permissions.
- **Documenting Data Origins**
  - Details of the collection process.
  - Methods used to clean and curate the data.
  - Pre-processing and transformation to the data.
- **Cataloging:** Organization and documentation of datasets.
- Helpful for transparency, traceability and accountability.

# 15. Security and Privacy for AI Systems

- **Threat Detection**
  - **Example:** Generating fake content, manipulated data, automated attacks.
  - Deploy AI-based threat detection systems.
  - Analyze network traffic, user behavior, and other relevant data sources.
- **Vulnerability Management**
  - Identify vulnerabilities in AI systems: software bugs, model weaknesses...
  - Conduct security assessment, penetration testing and code reviews.
  - Patch management and update processes.
- **Infrastructure Protection**
  - Secure the cloud computing platform, edge devices, data stores.
  - Access control, network segmentation, encryption.
  - Ensure you can withstand systems failures.

## 15.1. Security and Privacy for AI Systems

- **Prompt Injection**
  - Manipulated input prompts to generate malicious or undesirable content.
  - **Implement guardrails:** Prompt filtering, sanitization, validation.
- **Data Encryption**
  - Encrypt data at rest and in transit
  - Manage encryption keys properly and make sure they're protected against unauthorized access.

## 15.2. Monitoring AI systems

- **Performance Metrics**
  - **Model Accuracy:** Ratio of positive predictions.
  - **Precision:** Ratio of true positive predictions (correct vs. incorrect positive prediction).
  - **Recall:** Ratio of true positive predictions compare to actual positive.
  - **F1-score:** Average of precision and recall (good balanced measure).
  - **Latency:** Time taken by the model to make a prediction.
- **Infrastructure monitoring** (catch bottlenecks and failures)
  - Compute resources (CPU and GPU usage).
  - Network performance.
  - Storage.
  - System Logs.
- Bias and Fairness, Compliance and Responsible AI.

## 15.3. AWS Shared Responsibility Model

- **AWS responsibility** - Security of the Cloud.
  - Protecting infrastructure (hardware, software, facilities, and networking) that runs all the AWS services.
  - Managed services like Bedrock, SageMaker, S3, etc...
- **Customer responsibility** - Security in the Cloud
  - For Bedrock, customer is responsible for data management, access controls, setting up guardrails, etc...
  - Encrypting application data.
- **Shared controls**
  - Patch Management, Configuration Management, Awareness & Training.

## 15.4. Secure Data Engineering - Best Practices

- **Assessing data quality**
  - **Completeness:** Diverse and comprehensive range of scenarios.
  - **Accuracy:** Accurate, up-to-date, and representative.
  - **Timeliness:** Age of the data in a data store.
  - **Consistency:** Maintain coherence and consistency in the data lifecycle.
  - Data profiling and monitoring.
  - Data lineage.
- **Privacy-Enhancing technologies**
  - Data masking, data obfuscation to minimize risk of data breaches.
  - Encryption, tokenization to protect data during processing and usage.
- **Data Access Control**
  - Comprehensive data governance framework with clear policies.
  - Role-based access control and fine-grained permissions to restrict access.
  - Single sign-on, multi-factor authentication, identity and access management solutions.
  - Monitor and log all data access activities.
  - Regularly review and update access rights based on least privilege principles.
- **Data Integrity**
  - Data is complete, consistent and free from errors and inconsistencies.
  - Robust data backup and recovery strategy.
  - Maintain data lineage and audit trails.
  - Monitor and test the data integrity controls to ensure effectiveness.

## 15.5. Generative AI - Security Scoping Matrix

**Risk Management** is the process of identifying, assessing, and mitigating security risks associated with Generative AI applications.
- Framework designed to identify and manage security risks associated with deploying GenAI applications.
- Classify your apps in 5 defined GenAI scopes, from low to high ownership.
  ![Generative AI - Security Scoping Matrix](/Images/Machine%20Learning/GenerativeAISecurityScopingMatrix.png)

# 16. MLOps

- Make sure models aren't just developed but also deployed, monitored, retrained systematically and repeatedly.
- Extension of DevOps to deploy code regularly.
- **Key Principles**
  - **Version control:** Data, code, models could be rolled back if necessary.
  - **Automation:** Of all stages, including data ingestion, pre-processing, training, etc...
  - **Continuous Integration:** Test models consistently.
  - **Continuous Delivery:** Of model in productions.
  - Continuous Retraining.
  - Continuous Monitoring.
