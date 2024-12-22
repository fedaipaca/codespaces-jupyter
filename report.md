# Hate Speech Detection System

## Introduction
This report provides a short but comprehensive overview of the steps taken to build a Turkish hate speech detection system. The project involves data acquisition, preprocessing, feature engineering, model training, evaluation, and addressing class imbalance. The goal is to develop a robust model that can accurately classify tweets into categories such as "nefret" (hate speech), "saldırgan" (offensive), and "hiçbiri" (none).

## Data Acquisition and Cleaning

### Loading and Cleaning Data
**Troff Dataset:**
- Loaded the Troff dataset from a TSV file.
- Cleaned the text by removing newline and carriage return characters.
- Saved the cleaned data to a CSV file for further use.

**Turkish Hate Speech Dataset:**
- Loaded the Turkish Hate Speech dataset from an Excel file.
- Combined multiple sheets into a single DataFrame.
- Cleaned the text by removing newline and carriage return characters.
- Saved the combined data to a CSV file for further use.

### Data Exploration
- Loaded the refined datasets and performed exploratory data analysis.
- Displayed the distribution of labels and sampled data to understand the dataset better.

## Data Preprocessing

### Label Mapping
- Mapped the labels in the Troff dataset to match the labels in the Turkish Hate Speech dataset:
  - 'non' -> 'hiçbiri'
  - 'prof' -> 'saldırgan'
  - 'grp', 'ind', 'oth' -> 'nefret'

### Merging Datasets
- Merged the Troff and Turkish Hate Speech datasets into a single dataset.
- Saved the merged dataset to a CSV file for further use.

### Text Cleaning
- Defined a function to clean the text by removing URLs, mentions, hashtags, punctuation, and extra white spaces.
- Applied the text cleaning function to the merged dataset.

### Label Encoding
- Initialized a Label Encoder and created a new column for storing encoded labels.

### Text Processing with Zemberek
- Started the JVM for Zemberek, a Turkish NLP library.
- Defined functions to lemmatize and preprocess the text using Zemberek.
- Applied the preprocessing function to the merged dataset.
- Shut down the JVM after preprocessing.
- Since Zemberek is almost outdated as package, the uptodate ones can be used instead of Zemberek. 

## Feature Engineering

### Text Vectorization
- Defined vectorizers with different n-gram configurations (unigram, bigram, trigram) for both CountVectorizer and TfidfVectorizer.
- Applied the vectorizers to the training, validation, and test data.
- Output the feature counts for each vectorized dataset.
- To keep model training fast with less resourse only unigram and bigram picked.

## Model Training and Evaluation

### Model Training
- Defined multiple machine learning models: RandomForest, XGBoost, LightGBM, and MLP.
- Trained the models on the vectorized training data.
- Evaluated the models on the validation and test data.

### Model Performance
The table below summarizes the performance of various models trained on different feature sets, evaluated using the F1 score on both validation and test datasets:

| Feature Set                | Model          | Validation F1 Score | Test F1 Score |
|----------------------------|----------------|----------------------|---------------|
| CountVectorizer_unigram    | RandomForest   | 0.802659             | 0.790685      |
| CountVectorizer_unigram    | XGBoost        | 0.806404             | 0.791920      |
| CountVectorizer_unigram    | LightGBM       | 0.784832             | 0.771557      |
| CountVectorizer_unigram    | MLP            | 0.792583             | 0.787964      |
| CountVectorizer_bigram     | RandomForest   | 0.774422             | 0.758723      |
| CountVectorizer_bigram     | XGBoost        | 0.748393             | 0.732053      |
| CountVectorizer_bigram     | LightGBM       | 0.739237             | 0.723322      |
| CountVectorizer_bigram     | MLP            | 0.775466             | 0.764992      |
| TfidfVectorizer_unigram    | RandomForest   | 0.800019             | 0.787855      |
| TfidfVectorizer_unigram    | XGBoost        | 0.803589             | 0.787150      |
| TfidfVectorizer_unigram    | LightGBM       | 0.787426             | 0.771646      |
| TfidfVectorizer_unigram    | MLP            | 0.785723             | 0.787428      |
| TfidfVectorizer_bigram     | RandomForest   | 0.772213             | 0.759662      |
| TfidfVectorizer_bigram     | XGBoost        | 0.749505             | 0.730456      |
| TfidfVectorizer_bigram     | LightGBM       | 0.739699             | 0.720874      |
| TfidfVectorizer_bigram     | MLP            | 0.780740             | 0.774248      |

## Addressing Class Imbalance

### Resampling Techniques
- Applied various resampling techniques to address class imbalance:
  - SMOTE
  - Undersampling
  - SMOTEENN

### Resampling Results
The table below shows the F1 scores for different resampling techniques applied to address class imbalance:

| Resampling Technique | F1 Score |
|----------------------|----------|
| Original             | 0.794034 |
| SMOTE                | 0.799925 |
| Undersampling        | 0.638848 |
| SMOTEENN             | 0.167392 |

## Conclusion and Recommendation
Based on the results, the XGBoost model with CountVectorizer_unigram feature set achieved the highest F1 score on the test set (0.791920). Additionally, applying SMOTE as a resampling technique improved the F1 score to 0.799925, indicating better performance in handling class imbalance.

## Recommendation: 
Even though after changing the paramaters of the models to train in an optimal way the result changes, for this test the best choice for the hate speech detection system is to use the XGBoost model with CountVectorizer_unigram feature set, combined with SMOTE for resampling. This combination provides the highest F1 score, ensuring a balanced and accurate classification of hate speech.
