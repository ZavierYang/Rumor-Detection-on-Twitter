# Importance
This is a project from a NLP course (subject). **If you come to watch because of your course (subject) assignment, DO NOT just copy and paste this code or just modify the variables name. More importantly, DO NOT copy this article and the results otherwise your score is possible to be penalised.** 

In this course, there is a in-class Codalab competition which has 308 students participated. My final result is, 

    Public Leaderboard: 30 / 308 (10%)
    Private Leaderboard: 34 / 308 (11%)
  
# Rumor Detection Specification
The goal of this project is to build a binary classifier using the given dataset. Besides, combine different features to train the model in order to find the most effective feature set for prediction. Therefore, in the repository, I will use BERT model and train with different feature hypotheses to find which has the best result.

## Dataset
* [train,dev,test].data.jsonl: tweet data for rumour detection
* [train,dev].label.json: rumour labels for rumour detection

All data files are JSONL files. For these files, each line is an event: a list of tweets where the first tweet is a source tweet and the rest are reply tweets.
