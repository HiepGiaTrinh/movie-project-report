---
title: "Proposal"
date: 2026--01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Movie Recommendation System
## Movie Recommendation System Based on User Data

### 1. Executive Summary
The Movie Recommendation System is a user data-driven recommendation platform designed to provide a personalized experience for users. The system utilizes a combination of Content-Based Filtering and Collaborative Filtering.

Maintaining Machine Learning servers for real-time computations is extremely expensive. To resolve this issue, the entire model training and movie scoring process will be executed periodically in the background via SageMaker Processing Jobs and EC2. The results are stored on Amazon S3 and Amazon DynamoDB. The FastAPI Backend simply loads this data into memory upon startup to serve users instantly.

### 2. Problem Statement
_Current Issues_

Currently, online streaming services are booming, with the number of available movies and TV shows reaching tens to hundreds of thousands.

Core problems: 
- With too many choices, users are often overwhelmed by information and spend too much time (15–20 minutes on average) just browsing movie lists without finding a suitable option.

- Manually searching for movies and shows by keywords or genres does not accurately reflect the diverse and evolving preferences of users, leading to a degraded user experience.

- Users easily become frustrated and abandon the application if they do not find engaging content within the first few minutes.

Therefore, streaming services require an intelligent recommendation system capable of understanding user behavior and preferences, thereby providing personalized recommendations to help users save time and enhance their viewing experience.

_Solution_

The system uses Machine Learning algorithms to collect and analyze user behavioral traces, thereby discovering hidden patterns of preferences and content similarities. To ensure scalability as the number of users and data volume increase, the system cannot run solely on a local server but needs to leverage the power of cloud computing infrastructure. By integrating AWS services, the system can store large amounts of interaction data, schedule periodic model training, and deliver recommendation results to the web application at high speeds.

### 3. Solution Architecture

_AWS Services Used_

- **Amazon SageMaker**: To train and deploy Machine Learning models.
- **Amazon EC2**: To run data processing and computational tasks.
- **Amazon S3**: To store large datasets and model results.
- **Amazon DynamoDB**: To store structured data with low latency.

_Component Design_
 

### 4. Technical Implementation
_Implementation Phases_

The project consists of 2 parts developed in parallel: building the movie streaming Web application and building the Machine Learning movie recommendation model.

1. **Infrastructure Provisioning and Data Preparation:** Set up the foundational platform for both the Web and Machine Learning components, finalize the data schema, and complete the processing of raw data sources.
    - _Web Part:_
        - Set up the project structure, configure the Docker environment, and establish basic CI/CD pipelines.  
        - Build the FastAPI Backend framework, design the schema for DynamoDB, and create tables on AWS.  
        - Design basic UI/UX, and build the Register/Login and Onboarding flows on Vite. 
    - _Machine Learning Part:_
        - Preprocess The Movies Dataset.
        - Develop a Data Pipeline to export cleaned data into Train/Validation/Test sets.

2. **Cost Calculation:** Use the AWS Pricing Calculator to estimate and adjust budgets.

3. **Feature Development:** Finalize features, build user scenarios on the Web, and successfully train the initial recommendation models.
    - _Web Part:_
        - Develop the movie details page, and build APIs to display metadata.
        - Build a complete Interaction Pipeline: capture events (such as `click`, `watch`, `rate`, `like`) from the Frontend and store them in the Interactions table on DynamoDB.
    - _Machine Learning Part:_
        - Build a **Popularity Ranker** model for guest users and a **Content-Based Recommender** for logged-in users.
        - Develop the core **Collaborative Filtering** model, converting interaction events into weighted scores.
        - Integrate the model into a SageMaker Endpoint to serve movie recommendation predictions.

4. **System Integration and Cloud Deployment:** Connect the Backend with the Machine Learning model, ensuring the system adheres to a Batch-First architecture on AWS infrastructure.
    - _Web Part:_
        - Integrate the Machine Learning model into the Backend process. Build a POST API to route requests from the Frontend to the model.
    - _Machine Learning Part:_
        - Automate the model re-training process.
        - Run experiments on Amazon SageMaker.

5. **Testing:** Review the entire system, fix bugs, and measure real-world performance. Optimize page load times and DynamoDB query speeds. Monitor AWS costs to ensure no background resources exceed the budget. 

_Technical Requirements_

- _Frontend:_ Use Vite with a basic understanding of EC2. Build the movie display interface and manage login states.
- _Backend:_ Build using FastAPI. Accurately handle authentication flows, manage interaction collection flows, and route recommendation scenarios based on user status.
- _Machine Learning:_ Develop in Python using the `implicit`, `scikit-learn`, `numpy`, and `pandas` libraries. Required to build the Implicit ALS model, along with a hybrid Weighted RRF algorithm.
- _Cloud - AWS:_ Amazon S3 to store raw datasets and model result files. Amazon DynamoDB as the primary database for the Web. Use Amazon SageMaker to automatically run the Re-train process.

### 5. Roadmap & Milestones
- _Pre-internship (Month 0):_ Planning, dataset preparation.
- _Internship (Months 1-3):_
    - Month 1: General research on AWS services.
    - Month 2: Build the streaming web application and the movie recommendation system.
    - Month 3: System testing and preparation for real-world deployment.
- _Post-deployment:_ Monitor performance, optimize models, and expand features.

### 6. Budget Estimation
Costs can be viewed on the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=f696e1b79bc7b7f905e25226ff5b9f3b0011c562)

_Infrastructure Costs_

- Amazon EC2: $3.87/month (1 t3.micro, 730 hours, 30 GB storage).
- Amazon SageMaker: $1.41/month (1 ml.m5.xlarge, 30 jobs, 10 mins/job).
- Amazon DynamoDB: $0.37/month (On-demand, 1 GB storage, 100,000 requests).
- Amazon S3 Standard: $0.13/month (5 GB storage, 2000 requests).

_Total:_ $5.78/month, $69.36/12 months.

### 7. Risk Assessment

_Risk Matrix_

- **Cloud Cost Blowout - High Impact, Medium Probability:** Accidentally initializing SageMaker Real-time Endpoints, forgetting to turn off EC2 servers after training, or failing to configure lifecycle rules for Amazon S3, causing old data versions to accumulate permanently and incur hidden storage fees.
- **Model Degradation - High Impact, Fair Probability:** The automated training process runs on noisy or insufficient interaction datasets, generating poor-quality models that automatically overwrite well-performing ones.
- **Internal Data Misalignment - High Impact, Low Probability:** Index mapping files do not match the model matrix, causing the system to recommend completely incorrect movies without triggering any error alerts.

_Mitigation Strategies_

- **Budget Guardrails & Automated Cleanup:** Do not grant permissions to run Real-time Endpoints. Set up AWS Budgets to send email alerts when costs reach 50% and 80%. Strictly enforce S3 Lifecycle Rules to automatically delete old file versions after 30 days.
- **Automated Review Gates:** A new model is only permitted for deployment if it meets 3 conditions:
    - The number of scored users exceeds 1000.
    - Performance metrics surpass the Popularity Baseline.
    - Accuracy does not drop by more than 5% compared to the current version.
- **Cross-Data Validation:** Add verification steps on the Backend to ensure all movie IDs generated by the model actually exist in the DynamoDB catalog.

_Contingency Plans_

- **Safe Fallback Mechanism:** If the influx of new users lacks sufficient data or the Machine Learning model fails to load, the Backend will automatically revert to the safest scenario: Recommending Top-Rated popular movies directly from DynamoDB to ensure the system never crashes.
- **Model Rollback:** Upon detecting poor-quality recommendations in the production environment, the administrator simply updates the S3 pointer to the previous model version and restarts the Backend; the system will recover instantly.

### 8. Expected Outcomes
_Technical Improvements:_ 

- Successfully construct a **Batch-first** processing architecture combined with a memory-loaded Backend, completely eliminating prediction latency compared to conventional model API calling systems.
- The system is capable of automatically updating user priorities by capturing implicit interactions (watch time, shares, likes/dislikes) and learning through a periodic feedback loop.

_Long-term Value:_ 

- Create a solid architectural foundation that optimizes AWS infrastructure costs. The user routing and implicit data processing solutions can be reused for future e-commerce, online education, or large-scale content platforms.