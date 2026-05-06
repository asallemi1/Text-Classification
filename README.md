# Text-Classification
End-to-end data analysis and machine learning project on business reviews using MongoDB and Python. Includes data preprocessing, predictive modeling, and explainable AI (XAI) techniques to interpret model decisions.

--- 

# Business Reviews Analysis & Prediction

This project combines data analysis, machine learning for sentiment analysis, and explainable AI (XAI) to study business review data.  
It covers the full pipeline from raw data storage in MongoDB to predictive modeling and model interpretation.

---

## Project Overview

The goal is to extract insights from review data and develop predictive models that can estimate rating sentiment based on the review text.

The project includes:

- 📊 Exploratory Data Analysis (EDA)
- 🧹 Data preprocessing and filtering
- 🤖 Predictive modeling
- 🔍 Model interpretability using XAI techniques

---

## Tech Stack

- Python
- MongoDB
- Pandas / NumPy
- Scikit-learn 
- XAI libraries (e.g., SHAP, LIME)

---

## Workflow

### 1. Data Collection & Storage
- JSON review data stored in MongoDB
- Query-based data extraction

### 2. Data Filtering
-  Top 3 US locations with most reviews  
-  Time range: 2008–2012  
-  Focus on "Overall" rating  

### 3. Data Preprocessing
- Data cleaning and transformation
- Feature selection
- Creation of a structured dataset for modeling

Additionally, a new **sentiment** feature was created based on the overall rating:

- **Positive** if rating ≥ 4  
- **Negative** if rating ≤ 2  
- **Neutral** otherwise

Due to strong class imbalance, only positive and negative classes were retained, and a class rebalancing was performed on the training set.

### 4. Text Preprocessing
- 
---

## Predictive Modeling

Several machine learning models are trained to intepreate the sentiment.

### Models explored:
- Linear models (baseline)
- Tree-based models
- Neural networks (deep learning approach)

### Example Neural Network Architecture:
- Convolutional layers for feature extraction
- Dense layers for classification
- Hyperparameter tuning using **Hyperband**

### Final Model Performance:
- Accuracy: ~82.5%
- Peak Accuracy: ~82.9%

### Regularization Experiment:
- Dropout applied to reduce overfitting
- Slight performance decrease observed (~77.6%)

### Additional Experiment:
- Grayscale input version (for image-based tasks)
- Performance drop (~71%), highlighting the importance of color features

---

## 🔍 Explainable AI (XAI)

To better understand model predictions, XAI techniques are applied:

### Methods used:
- **SHAP (SHapley Additive exPlanations)**  
- **LIME (Local Interpretable Model-agnostic Explanations)**  

### Objectives:
- Identify which features most influence predictions  
- Analyze local vs global model behavior  
- Improve transparency and trust in the model  

### Insights:
- Certain features strongly drive rating predictions  
- Model decisions can vary depending on location and time context  
- XAI helps detect potential biases and model weaknesses  

---

## 📊 Results

The project demonstrates:

- Strong predictive performance using optimized models  
- Trade-off between generalization and accuracy (dropout case)  
- Significant impact of feature representation (e.g., color vs grayscale)  
- The importance of interpretability in machine learning workflows  
