### Problem Statement

Reddit is a social media site, where users post images, news articles, and user-generated posts to sub-forums, called sub-reddits.  Users can then comment and vote on posts.  Each sub-reddit focuses on an individual topic, such as news, math, or sports.  There are communities dedicated to many different interests and topics.  

In this project, we will be attempting to deal with a fictional case in which Reddit has come to us to determine if we can use NLP to determine which sub-reddit a post belongs to, using the post title.  They have lost the sub-reddit classification for posts in the canoeing and table tennis sub-reddits and want us to create a model to classify these posts.  In this fictional situation, we were able to scrape a dataset of posts before the data-loss occurred, and therefore have both the post title and post sub-reddit.

To create the best possible model to determine which sub-reddit a post belongs to, we will create a variety of classification models.  These models will be judged on their accuracy on the test dataset that we will create.  The winning model can then be used to classify the posts that are missing their sub-reddit classification.

### Executive Summary

The first notebook “1_import_data” uses a function to pull in post data from Reddit’s API.  We will pull in roughly 1,000 posts per sub-reddit.  We will pull in multiple fields, but will only use the sub-reddit name and the post title.  These two fields will be saved into csv files to be used in the later notebooks.  Pulling in 1,000 posts per sub-reddit will ensure that we have balanced classes.

The second notebook “2_eda” will give us an idea of the data.  By using CountVectorizer, we can see that count of each word in the post titles.  For each sub-reddit we will compile these counts and see which words are used most frequently in the post titles of canoeing and table tennis posts.  CountVectorizer can be used to create a matrix will either all of the words in the posts or a matrix that removes common English words (the, and, a, if, etc.).  By removing these common stop-words, we will be able to have a clearer picture of the words used commonly in the posts.  The top 10 most common words in each sub-reddit’s titles are mainly specific to the sport, but both have many appearances of the word “paddle”.

The third notebook “3_modeling” will take the data we pulled in the first notebook and run classification models on the data.  To run these models, we will try out both the CountVectorizer and TF-IDF.  Each model will be optimized for accuracy, and will have its hyperparameters optimized using GridSearchCV.  Grid search will allow us to find the optimal values for each model.  We will consider the following models”:

•	Logistic Regression <br />
•	Multinomial Naïve Bayes <br />
•	Random Forest <br />
•	AdaBoost on Decision Tree <br />
•	Voting Classifier (using the three most accurate models) <br />

The model that gives us the most accurate test score will be selected to fill in the missing data for Reddit.

### Conclusions and Recommendations

All of our models outperformed the baseline accuracy of .50.  The least accurate model was the AdaBoost model (using TF-IDF) which gave us an accuracy of .87.  Four of our models scored above .9; Logistic regression (CVEC, Voting Classifier, Multinomial Naïve Bayes (TF-IDF), and Logistic Regression (TF-IDF).  In all cases, our models showed signs of overfitting.  Our models performed better on the training data than on the test data.

Using the test accuracy as our judging criteria, logistic regression with TF-IDF was our top performing model and should be used to model our data.  Using this model, Reddit will be able to classify 93% of the table tennis and canoeing posts correctly.  Using GridSearchCV, we tuned the models hyperparamters to optimize the model.  The model performed best using the below hyper-parameters:

•	English stop-words <br />
•	single word ngrams <br />
•	A vocabulary with no minimum dataframe requirements <br />
•	A vocabulary maximum of 20% <br />
•	A maximum of 2,250 features <br />

In our test data set, the model did a better job of predicting the tennis posts than the canoe posts.  Because we calculated the sensitivity and specificity in addition to the accuracy, if it turned out that Reddit cared more about predicting either tennis or canoeing correctly, we could select the models based on those metrics.
Although our models performed well, we could perform some additional research to refine our models.  We could attempt to remove “paddle” from our documents by adding it as a stop-word.  We could also attempt to try more AdaBoost models, using other base models.  Finally, we could attempt to run our models using the Hashing Vectorizer.
