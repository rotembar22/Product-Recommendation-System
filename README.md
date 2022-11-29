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
The company objective is to solve the client's business problem by creating a real-time recommendation engine. 

# Overview

Product recommender systems surface items available for purchase across web pages, mobile apps, emails, etc. One of the most popular methods used by retailers, recommendations guide visitors to products they are likely interested in, improving the discovery process and helping them find what they want more efficiently.
Today, retailers often have thousands (and sometimes millions) of products in their inventories, making it difficult for consumers to dig up exactly what they are looking for.
Powered by machine learning, a product recommender system is a technology used to suggest which products are shown to individuals interacting with a brand’s digital properties. 

<p align="center">
<img src="images/New Bitmap image.JPg" width=50% height=50% >
</p>

# Dataset Description
<p align="center">
<img src="images/Screenshot 2022-11-29 104349.jpeg" width=50% height=50% >
</p>

<br>**tid** -Unique transaction ID
<br>**date** -Date of transaction
<br>**daypart** --Time of day when order occurred
<br>**channel** -Indicates how a customer placed their order
<br>**items** -Sequenced receipt data. Read from left-to-right, this indicates the sequence in which the customer chose each item while placing an order. e.g., "X,Y,Z" would mean that the customer ordered X, then Y, then Z within a single transaction.

# Model Metric & Model Requierments

<br>• Recommendations will occur at the transaction level and will be derived from the raw outputs of your predictive model. When making an item recommendation,
known information would include: daypart, channel, and the first 1+ item(s) that the customer has already chosen.
<br>• A recommendation will be determined and shown immediately after the customer chooses their first item. Optionally, the recommendation is allowed to update as a
customer chooses more items. However, a customer should see no more than 5 unique item recommendations during their order.
<br>• A recommendation will consist of 3 items that are shown to the customer simultaneously on a screen. (The ordering of the 3 recommended items does not matter.)

<br>Therefore the metric **Hit-Rate** described as follows:
A “hit” would occur if a customer purchases an item that was recommended/

<p align="center">
<img src="images/Screenshot 2022-11-29 102451.jpeg" width=50% height=50% >
</p>

# Research

There are many approaches that can address our task. In this project, I decided to use the word2vec embedding model. Here is a quick list of related works that used word2vec  for recommendation systems (I didn't read all of them).

<p align="center">
<img src="images/Screenshot 2022-11-29 102510.jpeg" width=50% height=50% >
</p>

# Methodology

The system I built consists of two main components. An embedding model aims to learn the relationships between the products and, thus, the ability to represent them in the vector space. And a classification model that can produce the vector of probabilities for purchasing each product.
 


<p align="center">
<img src="methodology.jpg" width=50% height=50% >
</p>


The model pipeline is described as follows:
1. Pre-processing- converting from unsupervised to semi-supervised by creating new instances while the target feature for each product is one of the following purchased products.
2. One-Hot Encoding for the categorical features
3. Train-Test split -an important note is that we will train only on the train set in the embedding model. It is easy to make a mistake by train embedding the model on all the data and encoding only the train, which causes data leakage.
4. Train a word2vec embedding model
5. Train Logistic Regression classifier.
6. We will extract the probability vector from the Logistic Regression classifier and suggest 3 to 5 top products for the test instances.

# Conclusions

**Model results:**
* Hit-rate results: 66.3%
* Hit-rate per item: 49.1%
Word2vec learns the semantic relationship between the items.

**Future work:**
* Testing different methodologies, for example, reinforcement learning, unsupervised(Matrix Factorization), and Cosine similarity.
* Model optimization: embedding vector size, regularization (L1/L2), classifier (SVM).
* Top 4&5 threshold tuning with a validation set.







