# Groove AI

![Cover Image](https://pbs.twimg.com/profile_banners/1871008187677372416/1734919044/1500x500)

### Table of Contents

- [Introduction](#introduction)
- [Requirements](#requirements) 
- [Installation](#installation)
- [Groove AI App](#groove-ai-app)
- [Architecture](#architecture)
- [Flow Chart](#flow-chart)
- [Prototype](#prototype)
    - [Feature Extraction](#feature-extraction)
    - [Principal Component Analysis](#principal-component-analysis)
    - [Dimensionality reduction](#dimensionality-reduction)
    - [Classification](#classification)
        - [K-nearest neighbors (KNN)](#k-nearest-neighbors-knn)
        - [Logistic Regression](#logistic-regression)
- [Python package groove_ai](#python-package-groove-ai)
    - [*feature*](#feature)
    - [*svm*](#svm)
    - [*acc*](#acc)
- [Results](#results)
- [Conclusion](#conclusion)
- [License](#license)

## Introduction

Music is categorized into subjective categories called genres. With the growth of the internet and multimedia systems, applications that deal with musical databases have gained importance, and the demand for Music Information Retrieval (MIR) applications has increased. Musical genres have no strict definitions and boundaries, as they arise through a complex interaction between public perception, marketing, historical, and cultural factors. **Groove AI** is a web application designed to classify music into genres with advanced AI algorithms.

## Accuracy

| Classifier              | Testing Accuracy |
|:-----------------------:|:----------------:|
| K-Nearest Neighbors    | 53%              |
| Logistic Regression    | 54%              |
| SVM Linear Kernel      | 52%              |
| SVM RBF Kernel         | 12%              |
| SVM Poly Kernel        | 64%              |
| **SVM Poly Kernel (6 classes)** | **85%** |

## Requirements

- Django (1.11)
- Numpy (1.12.1)
- Scikit-Learn (0.18.1)
- Scipy (0.19.0)
- Python-Speech-Features (0.5)
- Pydub (0.18.0)

## Installation

1. Clone the repository:
    ```bash
    git clone https://github.com/your-repo/groove-ai.git
    ```
2. Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3. Migrate database:
    ```bash
    python manage.py migrate
    ```
4. Run the server:
    ```bash
    python manage.py runserver
    ```

*The app will run on* `localhost:8000`

## Groove AI App

Our web application is built with the Django framework. It utilizes a trained `Poly Kernel SVM` for determining the music genre. Each HTTP request is routed through the URL dispatcher (`urls.py`) to specific views in `views.py`. Rules are defined to handle both HTTP POST and GET requests, allowing for music uploads and genre predictions.

Uploaded music files are stored temporarily in the database and deleted once the genre classification is complete. The app employs the `joblib` module from `sklearn.externals` to save and load trained classifiers. The trained classifier is saved on disk and loaded dynamically during operation.

The frontend handles user interactions via GET and POST HTTP requests. Uploaded files are converted to **BLOB (binary large objects)** by JavaScript and sent to the server. Once processed, the predicted genre labels are returned as JSON data.

The app incorporates the custom `groove_ai` Python package for feature extraction and classification.

## Prototype

Before building the Python-based web application, we developed a prototype in MATLAB to test various machine learning algorithms.

### Feature Extraction

MFCC (Mel Frequency Cepstral Coefficients) is used as the primary feature for genre classification. To avoid errors such as NaN or infinity, invalid values in the feature array were replaced with zeroes.

### Principal Component Analysis

PCA is applied to reduce the dimensionality of the feature set while retaining 90% of the variance. The MATLAB implementation demonstrated effective dimensionality reduction.

### Dimensionality Reduction

Mean and upper diagonal variance of MFCC coefficients are used to further reduce the feature vector size, resulting in a 104-dimensional vector.

### Classification

#### K-Nearest Neighbors (KNN)
KNN assigns a class to a data point based on the majority class of its k nearest neighbors in feature space.

#### Logistic Regression
Logistic Regression employs a sigmoid hypothesis function to predict class probabilities. For multi-class classification, a one-vs-all approach is used.

## Python Package *groove_ai*

The custom `groove_ai` package comprises the following modules:

### *feature* 

Handles MFCC feature extraction with functions such as:

- `extract(file)`: Extracts features from a file.
- `extract_all(audio_dir)`: Processes all files in a directory.
- `extract_ratio(train_ratio, test_ratio, audio_dir)`: Splits features into training and testing sets.

### *svm*

Manages SVM-based classification:

- `poly(X, Y)`: Trains an SVM with a polynomial kernel.
- `fit(training_percentage, fold)`: Trains and returns an SVM classifier.
- `getgenre(filename)`: Returns the predicted genre for a file.
- `getgenreMulti(filename)`: Returns multiple genre labels based on probabilities.

### *acc*

Calculates classification accuracy:

- `get(res, test)`: Compares predicted and actual labels to calculate accuracy.

## Conclusion

We explored various machine learning algorithms to achieve optimal accuracy. The **SVM with a polynomial kernel** achieved an accuracy of `85%` for six selected genres: classical, hip-hop, jazz, metal, pop, and rock. Multi-label classification based on genre probabilities is also supported for songs exhibiting features of multiple genres.

## License

This project is licensed under the MIT License.
