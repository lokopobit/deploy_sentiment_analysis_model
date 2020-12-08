# Introduction

This repository builds a model to classify film reviews as positive or negative, that is, sentiment analysis. The model used is a very simple LSTM neural network architecture defined with Pytorch library. The model is trained and deployed using sagemaker service of Amazon Web Services (AWS).

### Jupyter notebook workflow at a glance

The main code is main\_dsam\_mle.ipynb (dsam stands for deploy sentiment analysis model).

1. Download the dataset.
2. Prepare the data.
3. Upload de processed data to S3.
4. Train a chosen model.
5. Test the trained model.
6. Deploy the trained model.
7. Use the deployed model.
8. Create custom inference code.
9. Create a web app.


### Jupyter notebook workflow detailed

1. Download the dataset: the [IMDB dataset](http://ai.stanford.edu/~amaas/data/sentiment/ "IMDB dataset")    
2. Prepare the data: 
	1. Preprocess the data:
		1. Read the reviews and combine them into a single structure.
		2. Split the dataset into training set and testing set.
		3. Combine the positive and negative reviews and shuffle the resulting.
		4. Remove html tags and stopwords, convert to lower case and tokenize the reviews. Finally, transform the words appearing in the reviwes into integers.
	2. Transform the data from its word representation to a bag-of-words feature representation. Tme most frequent words will be represented with an integer and the infrequent with 1. The working vocabulary will be fixed to 5000. Finally, transform the word appearing in the reviews into integers.
3. Upload the processed data to S3: the data must be uploaded to S3 in order for our training code to access it. The data must be csv formatted with the first column the response variable. After creating a sagemaker.Session() object call the method upload\_data(). The local data path and a valid bucket name and S3 path are the arguments of the upload\_data method. 
4. Train a chosen model. A model in the SageMaker framework compirses three objects: Model Artifacts, Training Code and Inference Code. The training code is allocated in /train/model.py. We load a bit of data to check that our train() function is performing well. Finally we build aout sagemaker.pytorch.Pytorch() object and call the method fit. The Pytorch() object arguments are: the training dir and script name, the sagemaker role, the framework version, the number of instances and type, the python version and finally the hyperparameters that will be passed to the model. The fit() method can recieve the training and validation datasets, for instance. 
5. Test the trained model. We will be testing this model by first deploying it and then sending the testing data to the deployed endpoint.
6. Deploy the trained model. To deploy a model use the method deploy() on the Pytorch object. The parameters to this method are the number of instances and the type. For the instance to deploy de model it must load the model artifacts created during training. In order for sagemaker to do this a function which loads the saved model must be provided in the train.py script; this functions is called model\_fn(). When the predict() method of the object Pytorch() is used top predict an observation the built-in inference code imports the model\_fn() function from train.py.
7. Use the deployed model. We can read in the test data and send it off to our deployed model to determine how accurate our model is. If we want a raw review to be sent to the endpoint it must be pre-processed first. Finally, remember to delete the endpoint with de method delete\_endpoint().
8. Create a custom inference code. In the previous step a sagemeaker built-in inference code was used for prediction. We can instead create some custom inference code (serve/prdict.py) so that we can send the model a review which has not been processed and have it determine the sentiment of the review. When deplying a PyTorch model in SageMaker four functions must be provided to the inference container: model\fn(), input\_fn, output\_fn() and predict\_fn(). Finally, build the PyTorch model given the model artifacts using sagemaker.pytorch.PyTorchModel class.
9. Create a web app to access out model using lambda functions and an API Gateway.


### Changes between old sagemaker version (1) and latest (2)

### Reviewer comments

Further Resource: Keras has a neat API for visualizing the architecture, which is very helpful while debugging your network. pytorch-summary is a [similar project](https://github.com/sksq96/pytorch-summary/ "similar project") in PyTorch.

Further, AWS Sagemaker recently got a [new update](https://aws.amazon.com/es/blogs/machine-learning/amazon-sagemaker-now-comes-with-new-capabilities-for-accelerating-machine-learning-experimentation/ "new update"). It lets you find and evaluate the most relevant model training runs from the hundreds and thousands of your Amazon SageMaker model training jobs.

### TODO list

Check custom inference code accuracy.