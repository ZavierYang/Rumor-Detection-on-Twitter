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

An event looks like the following. Here the event consists of just 2 tweets, one source tweet and a reply tweet.

    [
        {
        "created_at": "Wed Jan 07 12:01:03 +0000 2015",
        "id_str": "552797058990870528",
        "in_reply_to_user_id_str": null,
        "text": "Spread this cover in solidarity with the victims at Charlie Hebdo. Don't let the
            sword conquer the pen. http:\/\/t.co\/XVkPPbkLhn",
        "user": {
            "id": 11345012,
            "id_str": "11345012",
            "name": "Ivo Vegter",
            ...
            },
        ...
        },
        {
        "created_at": "Wed Jan 07 17:39:24 +0000 2015",
        "id_str": "552882207426355201",
        "in_reply_to_status_id_str": "552797058990870528",
        "text": "@IvoVegter @pankajchandak #IMwithCharlieHebdo",
        "user": {
            "id": 1083066139,
            "id_str": "1083066139",
            "name": "Solid Item",
            ...
            },
        ...
        }
    ]
    
## Source
University of Melbourne COMP90042 Subject.

# Explanation of My Implementation and Result

## Applied Model
This report will apply BERT with a linear model for this project, and the reasons are as follows. Firstly, BERT has been pretrained so that fine-tune and linear model are the only things should be trained. Secondly, BERT can capture a longer context than LTSM. Finally, BERT does not necessary to do stemming, lemmatization, and stopwords removal since these processing will influence context. Qiao, Xiong, Liu & Liu’s (2019) experiment shows that regardless of whether stopwords are retained, the performance is almost the same.

## Feature Selection and Processing
There are many features in a tweet, but this report focuses on source tweet, reply tweet, and time. Moreover, applying various data processing to form different feature hypotheses in order to find out which have the best performance. The combinations and data processing are as the following table.

| Feature | Data processing |
|-----------------|:-------------|
| 1. Only source tweet | a. Remove nothing b. Remove URL c. Remove URL and hashtag |
| 2. Concatenate source and replies tweet | a. Remove nothing b. Remove @username c. Remove @username and URL d. Remove @username and URL and hashtag | 
| 3. Concatenate source and replies tweet by timeline | Same as 2. | 
| 4. Concatenate source and last 10 reply tweets by timeline | Same as 2. | 

When it comes to feature, training only source tweets or reply tweets which will have better performance. Besides, because a later reply has a higher probability of refuting rumor, the last 10 reply tweets will be concatenated with the source tweet. 

In terms of data processing, firstly, @username is deleted because it only indicates whom to reply to and has no meaning to the tweet. Secondly, an URL may be a factor in spreading rumor, but the URL itself does not have any context. Therefore, it may affect the contextual representation of other words. Finally, a hashtag is finally deleted because it may not be necessary to the source and reply tweet. Although hashtags may be indicators to distinguish whether it is a rumor, hashtags have no context for tweet content. However, because a hashtag may be a single word, it may have a meaning. As a result, hashtags will be the final text to be removed.

## Results Discussion and Conclusion
In this section, the F1 score will be used. The result is as the following table. The discussion will be conducted in two aspects with the hypotheses, namely development and testing results. Finally, we conclude the hypotheses by finding similarities between these two results. In the following discussion, 1, 2, 3, a, b, c, and d refer to Table in Feature Selection and Processing Section.

| | Dev F1 | Testing F1 |
|-----------------|:-------------|:-------------|
| 1-a | 0.8199 | 0.8085 |
| 1-b | 0.8197 | 0.7882 |
| 1-c | 0.8011 | 0.7978 |
| 2-a | 0.8242 | 0.8242 |
| 2-b | 0.8144 | 0.8197 |
| 2-c | 0.8287 | 0.836 |
| 2-d | **0.8309** | **0.8444** |
| 3-a | 0.8269 | 0.8023 |
| 3-b | 0.8247 | 0.7778 |
| 3-c | 0.8 | 0.809 |
| 3-d | **0.8306** | 0.8 |
| 4-a | 0.8273 | 0.8105 |
| 4-b | 0.8293 | 0.7853 |
| 4-c | 0.8154 | 0.8043 |
| 4-d | **0.8534** | 0.8 |

From development results, some hypotheses can be justified. Firstly, the highest F1 score in each feature 2 to 4 uses the d processing method. This proves that @username, URL, and hashtag are not useful for classification, as mentioned in the previous section. Additionally, training the sequence of replies can increase the F1 score, although the increase is not significant. Finally, the results show that getting the later reply tweets increases prediction performance since 4-d has the best F1 score

In terms of testing results, the conclusion is somewhat different above. The first point is that the reply tweets sequence does not increase the performance. Secondly, d processing is not necessarily the best in features 2 to 4. Eventually, training later reply tweets does not increase the performance. The reason for this is that not every later reply tweet has a refuting rumor content. Therefore, there is a certain "luck" factor in getting the last 10 replies. That is why 4-d in development and testing results will have a considerable gap.

However, although there are some differences between development and testing results, some similarities can be found to infer that certain hypotheses are correct. Firstly, the results of training sources with reply tweets are generally better than training sources only. Secondly, the best results in development and testing are performing d processing, namely 4-d in development and 2-d in testing. Therefore, d processing is still effective.

To conclude, training sources with reply tweets without considering the time sequence, and removing @username, URL, and hashtag is the best choice because it has the most stable performance in dev and testing.

## References
Qiao, Y., Xiong, C., Liu, Z., & Liu, Z. (2019).
    Understanding the Behaviors of BERT in
    Ranking. arXiv preprint arXiv:1904.07531.

