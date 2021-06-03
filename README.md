
### Submission by:


# Objective

We aim to build a network that would:

1) Download the StanfordSentimentAnalysis Dataset. This dataset contains just over 10,000 pieces of Stanford data from HTML files of Rotten Tomatoes. The sentiments are rated between 1 and 5, where one is the most negative and 5 is the most positive.

2) Use "Back Translate", "random_swap", random_insert and "random_delete" to augment the data you are training on.

3) Train your model and achieve 60%+ validation/text accuracy. Upload your collab file on GitHub with readme that contains details about your assignment/word (minimum 250 words), training logs showing final validation accuracy, and outcomes for 10 example inputs from the test/validation data.


# Data

- StanfordSentimentAnalysis is a popular dataset used for benchmarking NLP related models. It comprises of movies reviews and sentiments associated with reviews manually labeled for nearly 25 classes. We decided to go for total five labels ["very negative", "negative", "neutral", "positive", "very positive"].
- Dataset has got train, test split of (8544, 2210). On exploring the dataset, it was found to be imbalanced dataset.
![alt](https://github.com/SachinDangayach/END2.0/blob/main/Session5/Images/imbalance.png)

- We created methods to augment the dataset as well as convert the dataset into a balanced dataset.
  - Augmentation methods
  - random_swap: The random swap augmentation takes a sentence and then swaps words within it n times, with each iteration working on the previously swapped sentence. Here we sample two random numbers based on the length of the sentence, and then just keep swapping until we hit n.
  - random_delete: As the name suggests, random deletion deletes words from a sentence. Given a probability parameter p, it will go through the sentence and decide whether to delete a word or not based on that random probability. Consider of it as pixel dropouts while treating images.
  - random_insert: A random insertion technique looks at a sentence and then randomly inserts synonyms of existing non-stopwords into the sentence n times. Assuming you have a way of getting a synonym of a word and a way of eliminating stopwords (common words such as and, it, the, etc.)
  - back_translate: This method uses google translations to first translate the text to random language and then translate it back to English and while doing so we see change in the text while the meaning is preserved. There is a catch here while calling this method is google allows around 450 apis calls only and afterward it blocks the api call. Hence this method couldn't be used for generating good amount of text.
  - Finally we augmented the train dataset to 17114 records while converting it to almost balanced dataset.
  ![alt](https://github.com/SachinDangayach/END2.0/blob/main/Session5/Images/balance.png)

# The Network / Model - Architecture

The network we decided to build is designed as follows:
1. We have got embeddings layer with input dimension as vocab size (~15K) and embeddings dimension of 100.
2. We used bidirectional RNN ( GRU) with 2 layers with input as embeddings from embeddings layer and hidden layer with 256 dimensions
3. Dropout with (p= 0.3)
4. Fully connected layer with input dimension as 512 (2 * hidden layer as its bidirectional RNN) and output of 5 dimension.

Also, we have used the glove pre trained embeddings here by loading the pretrained embeddings weight to embeddings layer. We didn't free this layer at let the gradient to flow till this layer.

The summary of the network looks like this:  

![Model Summary](https://github.com/SachinDangayach/END2.0/blob/main/Session5/Images/model1.PNG)


# The Network / Model - Loss Function

We went ahead with the cross entropy loss with Adam optimizer with learning rate of 1e-5 for 80 epochs and then 1e-4 for next 20 epochs

# The training

Trained the model for 100 epochs. Model started overfitting in most of the experiments.

### ACCURACY ACHIEVED - 43.64%
Tried multiple experiment by couldn't get more than test 45% accuracy.

Train Test Loss and Accuracy plots  
![Accuracy and Loss plots](https://github.com/SachinDangayach/END2.0/blob/main/Session5/Images/loss_acc.png)


### Model results on manually / test dataset

Model results on random text

![alt](https://github.com/SachinDangayach/END2.0/blob/main/Session5/Images/Manual_results.PNG)

Model results of test dataset

![alt](https://github.com/SachinDangayach/END2.0/blob/main/Session5/Images/TestDataset.PNG)
