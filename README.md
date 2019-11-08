
#Enron Email Exploration

## **This project consists analysis of enron email dataset , clustering and classifying**

### 1. Clustering

The task here is to cluster sentences which are part of the emails into two different categories:
 1. Actionable Item
 2. Non-Actionable Item
 

Actionable item => A sentence which asks someone to do something
example: "Please create an assignment and forward it by EOD"
--------------------
```
These are the steps that I have followed to achieve this task:
 1. Read subset of email dataset(10000 emails) and filter out all but important information , in my case I have only chosen the body of the email
 2. Each email is split into sentences and each sentence goes through a manual set of rules which will categorize it into either actionable_item or not. As part of this:
    a. I try to clean the texts by applying some static rules
    b. The sentence is extracted using spacy library, its class is assigned 
    c. Each sentence along with the email from which we got it and the sentence_number within email is preserved in a pandas dataframe, later saved.
 3. At the end of the splitting and clustering I have 100000+ sentence and about 10% of them are clustered as actionable_items 
 ``` 
 If the text meets any of the given conditions then I mark it as actionable_item
 1. `Verb|Adverb followed by 'YOU' and ending with a Verb again'`
 2. `If the text ends with ?`
 3. `If it contains time entity` (I have used Spacy to find entities)
 
 ----------------------------------
 
 Post Clustering the details are stored within actionable_items_by_rules.csv.the features of this dataset are:
 1. sentence	
 2. mail_index	
 3. order_in_mail	
 4. actionable_item
 
As going through the emails and tagging them as actionable or not is a tiresome task , I have relied on some rules. As part of coming up with the rules I have concentrated more on getting more items classified as actionable_items, didn't worry about false positives.

------------------------- 
Post this I have also tried to make some sense by clustering the dataset by KMeans.
    - Used email body instead of sentences 
    - Used lemmatized form of words to decrease dimensionality
    - Fitted the data to a TFIdfVectorizer.Used unigrams as features
    - As part of this , I have tried to get the best K . Using Elbow method
    - For a sample set of 2000 emails, Optimal K was at 5

I visualized the top 25 important terms from all the clusters.  

----------------------------


To set things up:

1. Download the Dataset : https://www.kaggle.com/wcukierski/enron-email-dataset
2. Install requirements by typing
    `pip install -r requirements.txt`
3. Then type `jupyter notebook`   

Future Tasks:
1. Verify if any of the cluster has majority of actionable_items in them
2. If yes then we can derive more actionable_items from that cluster

------------------------------
 
 
 ### 2. Classification
 
 Steps that I followed before creating a model:
 
 - The quality of the clustering done using manual rules has proven to be not very effective as the a lot of sentences provided in actions.csv  were not clustered properly
 - I have collated actions.csv and non action items from my rules
 - Based on actions.csv I have added some more rules in code
 - Later I have manually corrected the labels for many to come up with the final_classifier_data.csv
 - From analysis I have found out that roughly 16% of sentences are clustered into action_items.
 - actions.csv only contained 1250 samples and assuming that it is 16% , I have sampled about 6500+ non action items from my clustered data
 - Post this I have done some more cleaning of text 

 I have used tensorflow keras layers to come up with a model , the architecture is fairly straightforward . there is one embedding layer , one dense layer and a output layer 
  
References:
    - https://www.tensorflow.org/tutorials/text/text_classification_rnn
    - https://www.kaggle.com/jamestollefson/enron-network-analysis
    - https://towardsdatascience.com/how-i-used-machine-learning-to-classify-emails-and-turn-them-into-insights-efed37c1e66
    - https://www.scikit-yb.org/en/latest/api/cluster/
    - http://web.science.mq.edu.au/~rdale/publications/papers/2010/naaclhlt2010-final.pdf
    
