# Text-Classification & Sentiment Analysis

The project focuses on **sentiment classification of hotel reviews** using multiple Natural Language Processing (NLP) approaches, including:

* Lexicon-based models
* Pre-trained transformer models
* Classical Machine Learning techniques

The project also integrates **MongoDB** for data storage and management, enabling efficient querying and exploration of the review dataset. Several analytical queries were performed on the MongoDB collections, and their description can be found in the `Readme_query.md` file.

The goal is to compare different text classification strategies, evaluate their effectiveness on a custom review dataset, and assess the reasoning behind model predictions through **Explainable AI** techniques.

---

# Project Overview

The project analyzes hotel reviews collected from multiple U.S. locations and performs:

* Text preprocessing and cleaning
* Binary sentiment classification
* Embedding generation
* Model comparison
* Explainability analysis

The study compares traditional NLP approaches with modern transformer-based architectures in order to evaluate trade-offs between interpretability and predictive performance.

---

# Dataset Description

The original dataset was provided by the university professors and consists of several JSON files containing hotel reviews.

## Available Fields

| Field            | Description                      |
| ---------------- | -------------------------------- |
| `Ratings`        | Numeric ratings from 1 to 5      |
| `AuthorLocation` | City where the review was posted |
| `Title`          | Review title                     |
| `Author`         | Review author                    |
| `ReviewID`       | Unique review identifier         |
| `Content`        | Review text                      |
| `Date`           | Review date                      |

## Time Range

| Information    | Value             |
| -------------- | ----------------- |
| First review   | August 1, 2001    |
| Last review    | September 9, 2012 |
| Selected years | 2008 – 2012       |

## Selected Locations

The project retained the three U.S. locations with the highest number of reviews after performing:

* Location normalization
* City/state extraction
* U.S. state validation
* Review counting by location

---

# Sentiment Labeling

The ratings were converted into **sentiment labels**.

## Label Definition

| Rating | Sentiment         |
| ------ | ----------------- |
| `≥ 4`  | Positive          |
| `≤ 2`  | Negative          |
| `= 3`  | Neutral (removed) |

## Class Distribution

| Sentiment | Percentage |
| --------- | ---------- |
| Positive  | 71.5%      |
| Negative  | 15.3%      |
| Neutral   | 13.2%      |

To address class imbalance, **down-sampling** was applied.

Final dataset size:

| Metric        | Value |
| ------------- | ----- |
| Total reviews | 4325  |

---

# Text Preprocessing Pipeline

The preprocessing stage included:

* URL removal
* Hashtag and mention removal
* Punctuation cleaning
* Tokenization
* Stop-word removal
* Lemmatization
* Stemming

This preprocessing pipeline was designed to standardize textual data before feature extraction and model training, and was applied exclusively to the Random Forest architectures.

---

# Models Evaluated

The project compares several sentiment analysis approaches.

## Lexicon-Based Models

### VADER

Lexicon-based sentiment analysis model based on polarity scores.

| Metric      | Score |
| ----------- | ----- |
| Specificity | 0.55  |
| Sensitivity | 0.99  |

### TextBlob

Rule-based sentiment analysis framework returning polarity values.

| Metric      | Score |
| ----------- | ----- |
| Specificity | 0.41  |
| Sensitivity | 0.99  |

---

## Pre-trained Models

### Flair

Contextual NLP framework using pre-trained sentiment classification models.

| Metric      | Score |
| ----------- | ----- |
| Specificity | 0.64  |
| Sensitivity | 0.97  |

### BERT

Transformer-based language model introduced by Google.

| Metric      | Score |
| ----------- | ----- |
| Specificity | 0.97  |
| Sensitivity | 0.93  |

---

## Machine Learning Models

### Random Forest + TF-IDF

Traditional Machine Learning pipeline using TF-IDF embeddings.

| Metric      | Score |
| ----------- | ----- |
| Specificity | 0.76  |
| Sensitivity | 0.78  |

### Random Forest + Word2Vec

Embedding-based representation using semantic word vectors.

| Metric      | Score |
| ----------- | ----- |
| Specificity | 0.91  |
| Sensitivity | 0.93  |

---

# Explainable AI

The project includes several Explainable AI techniques to interpret model predictions.

## Global Feature Importance

Random Forest feature importance was used to identify the most influential words in classification decisions.

### Example Important Features

| Positive-oriented words | Negative-oriented words |
| ----------------------- | ----------------------- |
| great                   | dirty                   |
| comfortable             | never                   |
| clean                   | worst                   |
| helpful                 | told                    |
| friendly                | said                    |

---

## SHAP Analysis

SHAP values were used to provide global explanations for model behavior.

<img width="1938" height="1516" alt="image" src="https://github.com/user-attachments/assets/5edec3c1-908a-4308-9edf-37c6aa06fbf4" />

Examples:

* Positive SHAP values for words such as `"great"` indicate contribution toward positive predictions.
* Negative SHAP values for words such as `"worst"` indicate contribution toward negative predictions.

---

## LIME Analysis

LIME was applied for local interpretability to explain individual predictions generated by both the Random Forest and Flair models. This enabled the inspection of token-level contributions for single reviews and provided a comparative analysis of the different decision-making behaviors of traditional machine learning and transformer-based architectures.

<img width="2820" height="1544" alt="image" src="https://github.com/user-attachments/assets/9466b9e4-8a05-4341-a6eb-f20d9e76568e" />

<img width="2696" height="1518" alt="image" src="https://github.com/user-attachments/assets/389f8e98-2539-4d89-b7d3-b243b6449872" />

---

# Technologies Used

| Category                | Technologies                        |
| ----------------------- | ----------------------------------- |
| Programming Language    | Python                              |
| Data Processing         | Pandas, NumPy                       |
| NLP                     | NLTK, TextBlob, Flair, Transformers |
| Machine Learning        | Scikit-learn                        |
| Embeddings              | TF-IDF, Word2Vec                    |
| Explainability          | SHAP, LIME                          |
| Database                | MongoDB                             |
| Development Environment | Jupyter Notebook                    |

---

# Future Improvements

Possible future extensions include:

* Multi-class sentiment classification
* Fine-tuning transformer models
* Cross-domain generalization
* Larger-scale datasets
* Advanced explainability techniques
* Hyperparameter optimization

---

# License

This repository is intended for academic and educational purposes only.


