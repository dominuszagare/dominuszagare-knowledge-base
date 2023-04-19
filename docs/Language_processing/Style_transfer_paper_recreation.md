# Creating a style transfer neural network

The goal of this project is to try to replicate results from a paper, where text written in one style is transformed into text written differently while retaining the meaning. More specifically we will be creating two neural networks that can transform positive Yelp reviews into negative Yelp reviews and vice versa. The paper we will be recreating is [A Dual Reinforcement Learning Framework for Unsupervised Text Style Transfer](https://arxiv.org/pdf/1905.10060.pdf).


## Classifying reviews as positive or negative

Before we can even start training our neural networks we need to classify the reviews as positive or negative. We can do this by using a simple neural network. We will be using a simple LSTM model with one hidden layer. The input to the model will be a sequence of numbers representing the words in the review. The output of the model will be a single number between 0 and 1. This number will represent the probability that the review is positive. We will be using the binary cross entropy loss function and the [Adam optimizer](https://arxiv.org/pdf/1412.6980.pdf).

### Data

For this example we will be using Yelp reviews dataset. The reviews considered positive are those with a rating of 4 or 5 and the negative reviews are those with a rating of 1 or 2. The reviews with a rating of 3 are ignored. The dataset contains 266041 positive and 177218 negative reviews, but we reduced it to 2000 positive and negative reviews for training, 500 positive and 500 negative reviews for testing, another 500 positive and 500 negative reviews for validation. The dataset can be downloaded from [here](https://www.kaggle.com/yelp-dataset/yelp-dataset).

### Training the classifier of positive and negative reviews

![clasificator training](./traning_my_casificator.png)

## Classifying content preservation

 Traning models from scratch is time consuming and expensive. So we use pretrained models found in , [Gensim](https://radimrehurek.com/gensim/) and [spaCy](https://spacy.io/).



## Training the style transfer neural network

Once we pretrained our clasificators we can start training our style transfer neural network. We will use our clasificators as a reward function. This will hopfuly teach the network to transform the reviews into the desired style. The reward function will be the following: ...



