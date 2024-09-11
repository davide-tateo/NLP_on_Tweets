# NLP_on_Tweets

## INTRODUCTION
On the 24th of February 2022, Russian troops crossed the border with Ukraine, prompting a group of countries, accounting for around 55% of the world GDP, to impose sanctions on Russia. These sanctions targeted Russian financial institutions, exports, and influential individuals. Russia retaliated by also banning exports of over 200 products and imposing limitations on foreign investments. This thorny geopolitical context has caused disruptions in the world supply chain, leading to economic consequences that also affected third countries. To address the challenge posed by the market’s active shifts, NLP techniques have gained increasing attention in the past decade as it is proven to be a powerful tool to support business decision-making and perform financial forecasting. Our project aims at understanding how NLP techniques applied on data scraped from social media can be exploited in the business sector. 

## OBJECTIVES OF THE PROJECT
This project will seek answers to 3 main research questions:

**RQ 1: What business insights can be derived from applying topic modelling techniques to Russian and Western war tweets, and do the topics discussed on Twitter accurately reflect the actual events occurring during the timeframe?**
Understanding the difference in topics found in Russian and Western tweets can shed light onto the information asymmetry that is occurring between the agendas served in the Russian and Western world. Combined with financial sentiment analysis, it enables the prediction of geopolitical tensions, thus improving risk management. 

**RQ 2: What financial insights can be derived from the sentiment analysis of the Russian and Western war tweets, and how can they be applied in practical situations?**
Financial Sentiment Analysis (FSA) is proven to be a powerful tool to support business decision-making and perform financial forecasting. The trends found can be used to adjust portfolio allocations, or even incorporated into high-frequency trading algorithms, helping investors predict market reactions to news and events and allowing for better timing of trades and investment strategies.

**RQ 3: Can the logistic regression model distinguish between Western and Russian war tweets, and what are its potential business applications?**
Exploring this question will reveal whether the discernible linguistic and thematic variances between Russian and Western war-related tweets are significant enough to be detected by automated classification, which can be used as a source classifier, especially if the data is then leveraged for business predictions.


## DATA DESCRIPTION
The dataset was sourced from Kaggle (Aleksandru, 2023) and originally divided into Russian and Western datasets, each with 36 columns. The Russian dataset contained 22,602 rows, while the Western dataset had 16,727. The data spans from February 22 to May 9, 2022. It was collected by scraping Twitter for popular Russian propaganda and Western analyst accounts. After preprocessing, the final dataset had 33,919 rows and 9 columns.

## DATA PREPROCESSING & EDA 
Relevant columns kept were tweet date, text, likes, and retweets. Two additional columns were added: tweet origin (Russian or Western) and the month. Dates were formatted consistently, and missing values were not found. Five duplicates in each dataset were removed. The Russian dataset had 5,875 more rows, and the majority of tweets came from March and April. Text preprocessing involved cleaning, tokenizing, and lemmatizing tweets, then applying a Bag of Words and TF-IDF representation to calculate word relevance.

## SELECTION OF MODELS
### Logistic Regression 
We employed a Logistic Regression model for binary classification, splitting the data 70-30 for training and testing. The 'saga' solver was chosen based on accuracy during testing and Grid Search. Although 'lbfgs' and 'liblinear' were considered, 'saga' performed best after tuning parameters, such as random_state (42), max_iter (100), and regularization (C=10). The model was evaluated using a confusion matrix, accuracy, precision, recall, F1-score, and AUC score.

### Topic Modelling with LDA 
Latent Dirichlet Allocation (LDA) was used for topic modeling. Hyperparameters were optimized via Randomized Search and manual tuning to maximize topic coherence, with scores of 0.48 for the Western dataset and 0.5 for the Russian, indicating moderate coherence.

### Dynamic Topic Modelling with BERTopic 
To analyze changes in topic prevalence over the four months, we used BERTopic, which integrates word embeddings with TF-IDF. This method, though computationally intensive, provided better interpretability compared to LDA by considering semantic context. We applied BERTopic's topics_over_time function to analyze topic shifts over time.

### Sentiment Analysis 
Sentiment analysis was conducted using the VADER library, which excels in social media due to its 7,500-word lexicon and negation rules. Sentiment scores were analyzed for correlation with retweets, likes, and exchange rates between the Ruble and key Western currencies.

## RESULTS
### Logistic Regression
The Logistic Regression model achieved an accuracy of 84.62%, correctly classifying the majority of instances. Precision for Western and Russian tweets was 82% and 87%, respectively, indicating low false positives. Recall scores were 82% (Western) and 86% (Russian), implying few false negatives. The F1-score, balancing precision and recall, was slightly lower for the Western class (82% vs. 86%). The confusion matrix revealed 915 false positives and 899 false negatives, indicating that the model performed better with Russian tweets, likely due to the class imbalance. The learning curve showed high accuracy on the training set, with a gap of around 15% between training and test accuracy, suggesting slight overfitting. The ROC curve yielded an AUC score of 0.92, indicating the model's ability to distinguish between Russian and Western tweets.

### Topic Modelling with LDA
LDA identified key topics in both datasets. For Western tweets, topic 1 (75.6%) focused on military operations and key events like the Azovstal Steel Plant. Topic 3 included references to locations like Bucha and Popasna. Topic 4 highlighted military tactics, while topic 2 centered around online discussions. For Russian tweets, topic 1 (72.5%) covered military operations and locations. Topic 2 focused on misinformation accounts like ‘GeromanAT’ and informal language. The Russian dataset was more focused on misinformation and polarizing content, while Western tweets covered a wider range of factual and objective topics.

### Dynamic Topic Modelling with BERTopic
BERTopic results aligned with LDA but added insights. Topic 1 (Russian) was centered around currency and prices, correlating with financial sentiment. Western topics focused on locations and military actions, peaking in frequency during the start of the war. Topic 5 (sanctions) surged in March when economic restrictions on Russia were announced. Western tweets remained more objective, while Russian tweets were more propagandistic, often mentioning “enemy” or “fascist regime.”

### Sentiment Analysis
Russian tweets had a less negative average sentiment (-0.036647) compared to Western tweets (-0.124199). Russian tweets displayed a higher percentage of positive sentiment, while most tweets were neutral. The correlation between Western and Russian daily sentiment was weak (0.0287), but noticeable sentiment shifts appeared around key events like NATO discussions. A weak positive correlation (0.12) was observed between Russian sentiment and ruble exchange rates, while Western sentiment had a negative correlation (-0.11). Engagement (retweets and likes) showed a slight negative correlation with sentiment scores, with more extreme sentiments receiving more engagement, especially in Western tweets.
