# Introduction

This repository builds a model to classify film reviews as positive or negative, that is, sentiment analysis. The model used is a very simple LSTM neural network architecture defined with Pytorch library. The model is trained and deployed using sagemaker service of Amazon Web Services (AWS).

### Jupyter notebook workflow at a glance

The main code is main\_dsam\_mle.ipynb.

1. Download the dataset.
2. Prepare the data.
3. Upload de processed data to S3.
4. Train a chosen model.
5. Test the trained model.
6. Deploy the trained model.
7. Use the deployed model


### Jupyter notebook workflow detailed

1. Download the dataset: the [IMDB dataset](http://ai.stanford.edu/~amaas/data/sentiment/ "IMDB dataset")    
2. Prepare the data: 
	1. Preprocess the data:
		1. Read the reviews and combine them into a single structure.
		2. Split the dataset into training set and testing set.
		3. Combine the positive and negative reviews and shuffle the resulting.
		4. Remove html tags and stopwords, convert to lower case and tokenize the reviews. Finally, transform the words appearing in the reviwes into integers.
	2. Transform the data from its word representation to a bag-of-words feature representation. Tme most frequent words will be represented with an integer and the infrequent with 1. The working vocabulary will be fixed to 5000. Finally, transform the word appearing in the reviews into integers.
3. Upload de processed data to S3.
4. Train a chosen model.
5. Test the trained model.
6. Deploy the trained model.
7. Use the deployed model


### Changes between old sagemaker version (1) and latest (2)