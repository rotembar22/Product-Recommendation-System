# Product Recommendation System

Designing a AI-based product recommendation system that offers the customer 3 to 5 products based on 1 product he intends to purchase.

![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/methodology.jpg?raw=true)

# Contents

[***Objective***](https://github.com/rotembaruch/Product-Recommendation-System#objective)

[***Overview***](https://github.com/uriaLevko/Disaster_response#overview)

[***Metrics Meaning***](https://github.com/uriaLevko/Disaster_response#Metrics_in_the_Disaster_response_domain)

[***Components***](https://github.com/uriaLevko/Disaster_response#Components)

[***Files***](https://github.com/uriaLevko/Disaster_response#Files)

[***Results_discussion***](https://github.com/uriaLevko/Disaster_response#Results_discussion)

[***Activation***](https://github.com/uriaLevko/Disaster_response#Activation)

<p align="center">
<img src="statis/dis3.jpg">
</p>
 
# Objective
The client’s objective is to recommend products that customers are likely to purchase.
The company objective is to solve the client's business problem by creating a real-time recommendation engine. The engine should be comprised of (1) a predictive model and (2) post-processing code which applies business rules to the raw model outputs.

# Overview

Product recommender systems surface items available for purchase across web pages, mobile apps, emails, etc. One of the most popular methods used by retailers, recommendations guide visitors to products they are likely interested in, improving the discovery process and helping them find what they want more efficiently.
Today, retailers often have thousands (and sometimes millions) of products in their inventories, making it difficult for consumers to dig up exactly what they are looking for.
Powered by machine learning, a product recommender system is a technology used to suggest which products are shown to individuals interacting with a brand’s digital properties. 


![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/images/New%20Bitmap%20image.JPg?raw=true)


# Dataset Description
![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/images/Screenshot%202022-11-29%20103824.jpeg?raw=true)

# Model Metric


# Components

There are three main components to this project.

1. ETL Pipeline - process_data.py:
* Loads the messages and categories datasets
* Merges the two datasets
* Cleans the data
* Stores it in a SQLite database

2. ML Pipeline - train_classifier.py, a machine learning pipeline that:
* Loads data from the SQLite database
* Splits the dataset into training and test sets
* Builds a text processing and machine learning pipeline
* Trains and tunes a model using GridSearchCV
* Outputs results on the test set
* Exports the final model as a pickle file
3. Flask Web App -
* classes data visualizations using Plotly in the web app.
* input massage to get class classification

# Files

[__Notebook 1__](/Files/ETL_Pipeline_Preparation.ipynb) : ETL

[__Notebook 2__](/Files/ML_Pipeline_PreparationF.ipynb) : ML Pipeline

[__Complete_Project__](/Files/disaster-response-pipeline-project): Full project


# Results_discussion

<p align="center">
<img src="statis/newplot (1).png" width=100% height=100% >
</p>

## Unballanced situation
looking at the image above, it's clear we are dealing with a highly imballanced dataset where only 3 classes has more then 20% minority class ratio, and many classes are pretty much all labeled as False.
*This is not an easy situation, and among the techniques to deal with it I would emphasize the following (there are meny any more):*
1. Data improvement:
 * Undersampling the Majority Class
 * Oversampling the Minority Class
 * Combine Data Undersampling and Oversampling
 * Cost-Sensitive Algorithms
 * Feature engineering
2. Threshold-Moving for Imbalanced Classification:
 * Converting Probabilities to Class Labels
 * Threshold-Moving for Imbalanced Classification
 * Optimal Threshold for ROC Curve
 * Optimal Threshold for Precision-Recall Curve
 * Optimal Threshold Tuning

At this point, no such techniques were used at this project, but I plan on improving the results in the near future.
## Precision vs. Recall

*by undertanding that we are dealing with a highly imbalanced dataset, some point needs to be taken in considiration when analazig the current results:*

1. We can't use accuracy as a metric as it will produce excelent results due to the imbalance
<p align="center">
<img src="statis/accuracy.PNG" width=50% height=50% title="99% accuracy with little to no effort">
</p>

2. According to the imbalance in class (positive or negative) to the topic, we will have to choose the right metric:
 * Precision: appropriate when minimizing false positives is the focus - appropriate classes that demand high resource allocation but are not life saving.
 * Recall: Appropriate when minimizing false negatives is the focus - apropriate classes that are highly life saving.
 * F-Measure: provides a way to combine both precision and recall into a single measure that captures both properties.
 
3. No single threshold can be activated  for all classes, and we will have to change threshold according to:
 * Class imbalance (positive or negative).
 * Metric selection (logical class determines the metric).
 * use of ROC curves as indicators.
4. Below there is a comparison of Threshold tuning with effect on the results. The impact is dramatic.
 
<img src="statis/beforeThreshup.PNG" title="Before Thresh Tuning" style="width: 350px;"/>
<img src="statis/afterThresh.PNG" title="After Thresh Tuning" style="width: 250px;"/> 

# Activation

1. Install the dependencies:
 If you are running this in your local environment, run conda install --file requirements.txt
 Or pip install -r requirements.txt to install the required python module dependencies
 
2. Instructions:
  Run the following commands in the project's root directory to set up database and model.
  
* To run ETL pipeline that cleans data and stores in database:<br>
  python data/process_data.py data/disaster_messages.csv data/disaster_categories.csv data/DisasterResponse.db
* To run ML pipeline that trains classifier and saves it as pickle file:<br>
  python train_classifier.py data/DisasterResponse.db models/classifier.pkl
  
 3.Running the Web App from the Project Workspace IDE:
 
 * Step 1: Type in the command line: python run.py
 * Step 2: Open another Terminal Window, Type: env|grep WORK
 * Step 3: In a new web browser window, type in the following: https://SPACEID-3001.SPACEDOMAIN where SPACEID & SPACEDOMAIN are shown in step 2.

 4. File Descriptions:
 * app-->
  - app -- template - master.html - main page of web app - go.html - classification result page of web app
  - app -- run.py - Flask file that runs app
 * data-->
  - disaster_categories.csv – file containing the categories
  - disaster_messages.csv – file containing disaster messages to be categorized
  - process_data.py – file containing data loaded code
  - DisasterResponse.db - database to save the cleaned and categorized data
 * models -->
  - train_classifier.py – file containing ML pipeline
  - classifier.pkl – model is saved in this pickle file
  
LICENSE: This project is licensed under the terms of the esri license product. There is no approval to copy or use this code without permission
