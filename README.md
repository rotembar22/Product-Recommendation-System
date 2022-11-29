# Product Recommendation System

# Contents

[***Objective***](https://github.com/rotembaruch/Product-Recommendation-System#objective)

[***Overview***](https://github.com/uriaLevko/Disaster_response#overview)

[***Metrics Meaning***](https://github.com/uriaLevko/Disaster_response#Metrics_in_the_Disaster_response_domain)

[***Components***](https://github.com/uriaLevko/Disaster_response#Components)

[***Files***](https://github.com/uriaLevko/Disaster_response#Files)

[***Results_discussion***](https://github.com/uriaLevko/Disaster_response#Results_discussion)

[***Activation***](https://github.com/uriaLevko/Disaster_response#Activation)


 
# Objective
The client’s objective is to recommend products that customers are likely to purchase.
The company objective is to solve the client's business problem by creating a real-time recommendation engine. The engine should be comprised of (1) a predictive model and (2) post-processing code which applies business rules to the raw model outputs.

# Overview

Product recommender systems surface items available for purchase across web pages, mobile apps, emails, etc. One of the most popular methods used by retailers, recommendations guide visitors to products they are likely interested in, improving the discovery process and helping them find what they want more efficiently.
Today, retailers often have thousands (and sometimes millions) of products in their inventories, making it difficult for consumers to dig up exactly what they are looking for.
Powered by machine learning, a product recommender system is a technology used to suggest which products are shown to individuals interacting with a brand’s digital properties. 


![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/images/New%20Bitmap%20image.JPg?raw=true)


# Dataset Description
![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/images/Screenshot%202022-11-29%20104349.jpeg?raw=true)

**tid** -Unique transaction ID
**date** -Date of transaction
**daypart** --Time of day when order occurred
**channel** -Indicates how a customer placed their order
**items** -Sequenced receipt data. Read from left-to-right, this indicates the sequence in which the customer chose each item while placing an order. e.g., "X,Y,Z" would mean that the customer ordered X, then Y, then Z within a single transaction.

# Model Metric & Model Requierments

• Recommendations will occur at the transaction level and will be derived from the raw outputs of your predictive model. When making an item recommendation,
known information would include: daypart, channel, and the first 1+ item(s) that the customer has already chosen.
• A recommendation will be determined and shown immediately after the customer chooses their first item. Optionally, the recommendation is allowed to update as a
customer chooses more items. However, a customer should see no more than 5 unique item recommendations during their order.
• A recommendation will consist of 3 items that are shown to the customer simultaneously on a screen. (The ordering of the 3 recommended items does not matter.)

Therefore the metric **Hit-Rate** described as follows:
A “hit” would occur if a customer purchases an item that was recommended/

![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/images/Screenshot%202022-11-29%20102451.jpeg?raw=true)


# Research

There are many approaches that can address our task. In this project, I decided to use the word2vec embedding model. Here is a quick list of related works that used word2vec  for recommendation systems (I didn't read all of them).

![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/images/Screenshot%202022-11-29%20102510.jpeg?raw=true)


# Methodology

The system I built consists of two main components. An embedding model aims to learn the relationships between the products and, thus, the ability to represent them in the vector space. And a classification model that can produce the vector of probabilities for purchasing each product.
 

![Seer Logo](https://github.com/rotembaruch/Product-Recommendation-System/blob/main/methodology.jpg?raw=true)

The model pipeline is described as follows:
1. Pre-processing- converting from unsupervised to semi-supervised by creating new instances while the target feature for each product is one of the following purchased products.
2. One-Hot Encoding for the categorical features
3. Train-Test split -an important note is that we will train only on the train set in the embedding model. It is easy to make a mistake by train embedding the model on all the data and encoding only the train, which causes data leakage.
4. Train word2vec embedding model
5. Encode each transaction by averaging the embedded vectors.
6. Train Logistic Regression classifier.
7. We will extract the probability vector from the Logistic Regression classifier and suggest 3 to 5 top products for the test instances.


# Files

[__Notebook __](/Product Recommendation System.ipynb)




# Theoretical Background

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
