# Steps
1.	Data Analysis
2.	Data Cleansing
3.	Feature Engineering
4.	Model Identification
5.	Train and validate the model 
6.	Predict the price for test.tsv data using the trained model

## Data Analysis
### A.	Numerical features
•	Price – This is our response that we need to predict
•	Shipping cost
### B.	Categorical features
•	Shipping cost: A binary indicator, 1 if shipping fee is paid by the seller and 0 if paid by buyer.
•	item_condition_id: The condition of the items provided by the seller
•	name: The item's name
•	brand_name: The item's producer brand name
•	category_name: The item's single or multiple categories that are separated by "\"
•	item_description: A short description on the item that may include removed words, flagged by [rm]

#### Price(Target Variable)
The median price of all the items in the training is about \$267 but given the existence of some extreme values of over \$100 and the maximum at \$2,009, the distribution of the variables is heavily skewed to the left. So let's make log-transformation on the price (we added +1 to the value before the transformation to avoid zero and negative values).

#### Shipping
The shipping cost burden is decently split between sellers and the sellers (55%) pay buyers with more than half of the items’ shipping fees.

#### Item Category
There are about 1,287 unique categories but among each of them, we will always see a main/general category firstly, followed by two more particular subcategories (e.g. Beauty/Makeup/Face or Lips). In addition, about 6,327 items do not have a category labels. 

#### Brand name
There are 4809 unique brand names in the training dataset.

#### Item Description
It will be more challenging to parse through this particular item since it's unstructured data.  We will strip out all punctuations, remove some english stop words (i.e. redundant words such as "a", "the", etc.) and any other words with a length less than 3:

## Cleansing Data
1. Missing value handling
2. Removing skewness
3. Stop words removal

## Feature Engineering
1. Name: We have used a maximum of 50K features. 
2. Category Name: We tokenized this field into General Category, Sub Category 1 and Sub Category 2
3. Shipping & Item Condition: We converted this to string and used.
4. Item Description: We considered 100K features.

## Model Identification
1. Identified the category of the problem statement (Regression)
2. Identified 2-3 algorithms to fulfil the requirement (Linear Regression, Ridge Regression, Recurrent Nueral Network)
3. Compared (Linear and Ridge) based on performance - and decided to go ahead with Ridge.
4. Ridge Regression uses L2 regularization technique which solves the problem of over-fitting (compared to Linear regression)
5. Ridge Regression works well when we have a lot of features, each of which contributes a bit in predicting (y-price)
  
## Train and Validate the model
We have used Ridge Regression model from scikit learn library to train the model. We then validated the model using K-Fold cross calidation technique.
In the k-fold cross validation method, all the entries in the original training data set are used for both training as well as validation. Also, each entry is used for validation just once. 
K-fold cross validation is performed as per the following steps:
1. Partition the original training data set into k equal subsets. Each subset is called a fold. Let the folds be named as f1, f2, …, fk . 
2. For i = 1 to i = k 
Keep the fold fi as Validation set and keep all the remaining k-1 folds in the Cross validation training set. 
Train your machine learning model using the cross validation training set and calculate the accuracy of your model by validating the predicted results against the validation set.
3. Estimate the accuracy of your machine learning model by averaging the accuracies derived in all the k cases of cross validation. 

We then used RMSLE(Root Mean Squared Logarithmic Error) to find the error rate.

# Potential shortcomings
1. We could do more word processing and cleaning up of data. This will be helpful for the feature extraction.
2. The model is not tested on any other data beyond the test data provided. 

# Possible improvements
1. We can try other algorithms like RNN(Recurrent neural networks)
2. We could use domain specific stopwords to derive better results, in addition to english stopwords.
3. Removal of outliers during cleasing data (0 and max values).
4. Considering Stochastic Gradient Descent(SGD) Regressor - since the number of values in the dataset is close to 1.4M
