title: "Know Thy Neighbor: An Introduction to Scikit-Learn and K-NN"

###Machine Learning

The algorithm learns from data.
Algorithms use data to create predictive models, classify unknown entities, discover patterns.

Usual work distribution

* 70% Clean and Standardize Data
* 20% Preprocess, Training, Validate
* 10% Analyze and visualize

### Scikit-Learn
Python machine learning package
Has built in datasets

Classification Example
Is a note a recipe?

Naive Bayes Classification

[Sci-kit Learn Algorithm Cheat Sheet](http://scikit-learn.org/stable/_static/ml_map.png)

####Tips

* Keep sample size high
* Keep feature set low

Having high feature set increases the risk of overfitting. 

###k-NN

* Simplest machine learning algorithm
* It is a lazy algorithm - it doesn't run computations on the dataset until you give it a new data point you are trying to test

###Problem 1
Have Apples, Oranges, and a mystery fruit in a group of fruits.
Set k-NN to 3 causes it to look at the 3 closest neighbors.
Majority vote

* Equal weight: Each kNN neighbor has equal weight
* Distance weight: Each kNN neightbor's vote is based on the distance

####Downsides of kNN
* Since there is a minimum training, there is a high computation cost

####Distance weighted vs equal weight
When the k-NN is set to the size of the training set, using equal weight will cause the graph to be completely classified as the majority.
