# Real-Time-Sentiment-Analysis-using-Spark

## Introduction:

Sentiment analysis is a natural language processing technique used to interpret and classify emotions in subjective data. Sentiment analysis is often performed on textual data to detect sentiment in emails, survey responses, social media data, and beyond. We use sentiment analysis on tweets from Twitter users that help understand the sentiment of a topic being discussed on the social media platform that could provide a sense of the public opinion based on various filters like region, country, time, etc. In our project, we implement various techniques to obtain meaningful insights from tweets. We have used frameworks like ​Kafka, Pyspark, Tweepy, the NLTK and Gensim python library, VaderSentiment, Elasticsearch and Kibana. ​Further into the project report, we will get into the details of how each of these frameworks help provide insights on twitter data​.

## Frameworks and their purpose

**Tweepy**:​ Tweepy is open-sourced, hosted on GitHub and enables Python to communicate with Twitter platform and use its API. Tweepy supports accessing Twitter via OAuth and provides access to the Twitter API. With tweepy, it's possible to get any object and use any method that the official Twitter API offers.(​ 6)
We use Tweepy to obtain a corpus of tweets to perform our sentiment analysis on.

**Kafka**:​ Kafka is an event streaming platform capturing data in real-time from event sources like databases, sensors, mobile devices, cloud services, and software applications in the form of streams of events; storing these event streams durably for later retrieval; manipulating, processing, and reacting to the event streams in real-time as well as retrospectively; and routing the event streams to different destination technologies as needed. Event streaming thus ensures a continuous flow and interpretation of data so that the right information is at the right place, at the right time.

Kafka combines three key capabilities so you can implement your use cases for event streaming end-to-end with a single battle-tested solution:
- To publish (write) and subscribe to (read) streams of events, including continuous import/export of your data from other systems.
- To store streams of events durably and reliably for as long as you want.
- To process streams of events as they occur or retrospectively.

Kafka is a distributed system consisting of servers and clients that communicate via a high-performance TCP network protocol.

**Servers**: Kafka is run as a cluster of one or more servers that can span multiple data centers or cloud regions. Some of these servers form the storage layer, called the brokers. To let you implement mission-critical use cases, a Kafka cluster is highly scalable and fault-tolerant: if any of its servers fails, the other servers will take over their work to ensure continuous operations without any data loss.

**Clients**: They allow you to write distributed applications and microservices that read, write, and process streams of events in parallel, at scale, and in a fault-tolerant manner even in the case of network problems or machine failures. Clients are available for Java and Scala including the higher-level Kafka Streams library, for Go, Python, C/C++, and many other programming languages as well as REST APIs.(​ 7)

We have used Kafka to capture tweets in real time with a producer and a consumer to read those tweets, define a sentiment metric and pass them to elasticsearch.

**Spark and Pyspark**:
Apache Spark is a unified analytics engine for large-scale data processing. It provides high-level APIs in Java, Scala, Python and R, and an optimized engine that supports general execution graphs. Spark uses Hadoop’s client libraries for HDFS and YARN.
PySpark, helps you interface with Resilient Distributed Datasets (RDDs) in Apache Spark and Python programming language. This has been achieved by taking advantage of the Py4j library. Py4J is a popular library which is integrated within PySpark and allows python to dynamically interface with JVM objects.Furthermore, there are various external libraries that are also compatible. Two of them used in our project are,
PySparkSQL:A PySpark library to apply SQL-like analysis on a huge amount of structured or semi-structured data. We can also use SQL queries with PySparkSQL. PySparkSQL is a wrapper over the PySpark core. PySparkSQL introduced the DataFrame, a tabular representation of structured data that is similar to that of a table from a relational database management system.
MLlib: MLlib is a wrapper over the PySpark and it is Spark’s machine learning (ML) library. This library uses the data parallelism technique to store and work with data.​ ​MLlib supports
many machine-learning algorithms for classification, regression, clustering, collaborative filtering, dimensionality reduction, and underlying optimization primitives.(​ 5)
Well in order to have the big data element in our project, we needed to have spark provide us RDDs to contain our tweets to work with. Since our sentiment analysis was all in python, we use Pyspark.

**Elasticsearch and Kibana:**
Elasticsearch is a distributed, RESTful search and analytics engine capable of addressing a growing number of use cases. As the heart of the Elastic Stack, it centrally stores our data for lightning fast search, fine‐tuned relevancy, and powerful analytics that scale with ease.
Kibana is a free and open user interface that lets us visualize our Elasticsearch data and navigate the Elastic Stack. With Kibana we can do anything from tracking query load to understanding the way requests flow through our apps.(​ 2)
For our real time sentiment analysis visualization, we pass our Kafka stream to elasticsearch to then let Kibana access our tweets on the elastic stack.

**VaderSentiment:**
VADER (Valence Aware Dictionary and sEntiment Reasoner) is a lexicon and rule-based sentiment analysis tool that is specifically attuned to sentiments expressed in social media. We use the SentimentIntensityAnalyzer function which takes sentences as inputs and gives an output in the following format.

**Input: VADER is smart, handsome, and funny.**

**Output: {'pos': 0.746, 'compound': 0.8316, 'neu': 0.254, 'neg': 0.0}**

We use the compound score, which is computed by summing the valence scores of each word in the lexicon, adjusted according to the rules, and then normalized to be between -1 (most extreme negative) and +1 (most extreme positive). This is the most useful metric if you want a single unidimensional measure of sentiment for a given sentence. Calling it a 'normalized, weighted composite score' is accurate.

## Topic Modelling

Topic modelling is one of the forms of unsupervised Clustering algorithm that segregates large corpus, while it preserves statistical relationships that help in classification. The goal is to uncover latent(hidden) variables that govern document semantics, and these hidden variables represent different abstract topics. It helps us to do the following:

➢ To discover hidden themes in the collection of data.
➢ Using classification, summarize the documents.

One of the most used techniques for topic modelling is Latent Dirichlet Allocation (LDA). With LDA models, the document is represented as a mixture of random latent topics, where every topic has been characterized by their distribution over words.
The innovation of LDA is in where the model uses Dirichlet priors (alpha) for document-topic and term-topic distributions. It allows for Bayesian Inference over a 3-level hierarchical model. The third layer is the distinguishing feature when compared to a simple Dirichlet multinomial clustering model.



 ![Screenshot 2021-05-28 at 5 41 52 PM](https://user-images.githubusercontent.com/29014647/120048175-0440d480-bfdc-11eb-9f20-2dab8e4db4a1.png)
 
 
 𝝰 = Dirichlet prior parameter of per document-topic distribution 𝞫​k =​ Dirichlet prior parameter of per topic-word distribution

𝝷​d =​ Document topic distribution for document d Z​d,n​ = Word topic assignment for W​d,n

W​d,n​ = Observed word i.e. nt​ h​ word in dt​ h​ document.

k= Number of topics

N= Number of words in the document 
d= Number of documents


**NLTK and Gensim for LDA:**

Gensim is a Python library for topic modelling, document indexing and similarity retrieval with large corpora. Its features include efficient multicore implementations of popular algorithms, such as online Latent Semantic Analysis (LSA/LSI/SVD), Latent Dirichlet Allocation (LDA), Random Projections (RP), Hierarchical Dirichlet Process (HDP) or word2vec deep learning.

NLTK stands for Natural Language Toolkit. This toolkit is one of the most powerful NLP libraries which contains packages to make machines understand human language and reply to it with an appropriate response. Tokenization, Stemming, Lemmatization, Punctuation, Character count, word count are some of these packages.(​ 3)

For the LDA sentiment analysis aspect we use the NLTK library for data preprocessing and we use the Gensim package to obtain an LDA model to which we pass our data.

In the implementation portion, we’ll discuss the flow of our project and when and where each of these packages come into picture.

## Implementation

We obtain 5000-10000 tweets and collect the text and hashtags present in the tweets using the tweepy package. The tweepy.Cursor function lets us use a query to obtain tweets containing a certain keyword or a timeframe, etc.
We have multiple use cases in which the queries differ. For both the implementation techniques, we use a keyword search to obtain tweets for a certain hashtag. Since there are multiple approaches to our problem, we’ll go one by one and describe the implementation of each method one by one.


**Part 1:**
We create a Spark Dataframe of the tweets and create a column with all the symbols removed to have clean text. We then bring in the nltk and gensim modules and download the corpora necessary for stemming and lemmatization and to tokenize the tweets.
The next step is to convert each document into the bag-of-words (BoW) format = list of (token_id, token_count) and pass that corpus to an LDA model imported from gensim along with a hyperparameter of the number of topics in topic modeling, this we have set to 5.Then, we pass the same corpus to the TF-IDF model to obtain the TF-IDF corpus and pass them to the LDA model.
Finally, we use the pyLDAvis module to visualize the LDA models from both the bag of words corpus and the TF-IDF corpus which plots the intertopic distance map and the top 30 most salient terms based on the relevance metric.
Relevance is denoted by λ, the weight assigned to the probability of a term in a topic relative to its lift. When λ = 1, the terms are ranked by their probabilities within the topic (the 'regular' method) while when λ = 0, the terms are ranked only by their lift.
The results for this model are shown in the results section.

**Part 2:**
In this alternative method, we have used Kafka to implement real time sentiment analysis. Our code bits have two components: Producer and Consumer.
Please note that this method works only with specific versions of the various components involved. These include Kafka 2.4.1, Pyspark 2.3.0 with Hadoop 2.7, Python 3.6 virtualenv and Elasticsearch 7.6.2.
Producer: Our Kafka producer, as the name suggests, produces a stream of tweets using Tweepy, stores it and ensures a continuous flow and interpretation of data so that consumer obtains tweets in batches.
We have two functions that we write here, the first to preprocess the text obtained from the tweepy api, clean it, keep the data fields we need and add the sentiment score as another field to our tweet stream, and the second being a Twitter Stream Listener which we named KafkaPushListener. We use the default Zookeeper producer host and port address and get the producer with the topic name we set for our project on our machine.

We finally obtain our stream in json format with the following lines

![Screenshot 2021-05-28 at 5 46 43 PM](https://user-images.githubusercontent.com/29014647/120048413-ad87ca80-bfdc-11eb-85b1-e31caf42efc6.png)

Consumer: Once the producer starts producing our tweets, we pass the necessary packages for our pyspark running hadoop to understand what’s necessary for our consumer. The packages being spark-streaming-kafka, spark-sql-kafka and elasticsearch-hadoop.
We then create a user-defined function to evaluate the sentiment of the tweet string we get.
The next function is called start_stream which takes a dataframe as input in append mode and sends it to elasticsearch (ES), which means our stream keeps getting sent to ES as the producer keeps generating tweets.
We then set the configuration for spark and define a schema based on the json produced by the producer.
After this, we get our logger and then store our kafka stream (with the bootstrap server option, older versions of kafka use the zookeeper) in a variable called kafkaStream, select the necessary fields and convert the timestamp into an elasticsearch compatible one. The sentiment score, dates and tweets are stored in a final data frame that is passed to the start_stream function which as its already mentioned, sends it to the elasticsearch stack.
Elasticsearch is accessed on our browser using **​localhost:9200/**
The advantage of elasticsearch and kibana for this project is to allow users to access the wide range of search querying and visualization that it provides.
Unfortunately, due to Kakfa log issues, we are not able to attach the results for this part of our project. We really did try a lot to debug them but they only tend to run by luck a couple of times.

## Results

**Part 1:**
Bing Liu sentiments are obtained for our tweets containing the hashtag to understand the overall sentiment of tweets in the first implementation
![Screenshot 2021-05-28 at 5 48 00 PM](https://user-images.githubusercontent.com/29014647/120048490-dc9e3c00-bfdc-11eb-8563-e841960c2bb4.png)

Since topic modeling using LDA doesn’t have a way to show us how it scores each word, the blackbox is visualized with the pyLDAvis package. The results are shown below.

![Screenshot 2021-05-28 at 5 48 32 PM](https://user-images.githubusercontent.com/29014647/120048519-ee7fdf00-bfdc-11eb-8108-6ca440b46f57.png)


We use this in use cases like seeing what other words might be along similar sentiments at a given point in time. For example, in Figure 2, we see that in Topic 2 when λ = 0.62, the words (they are actually hashtags) “​trumpisacriminal​”, “​biggestloosertrump​” and “​defundthepolice​” are hashtags that have similar sentiments and we can infer that people in favor of defunding the police are people who don’t support Trump.

**Part 2:**
VaderSentiment provides tweet sentiments that we add to our json that’s passed to the ElasticSearch stack. The stream looks like this.

![Screenshot 2021-05-28 at 5 50 14 PM](https://user-images.githubusercontent.com/29014647/120048609-2b4bd600-bfdd-11eb-8a51-f403618485b4.png)

## References

1) (​cjhutto/vaderSentiment: VADER Sentiment Analysis. VADER (Valence Aware Dictionary and sEntiment Reasoner) is a lexicon and rule-based sentiment analysis tool that is specifically attuned to sentiments expressed in social media, and works well on texts from​, n.d.)
https://github.com/cjhutto/vaderSentiment
2) (​Elasticsearch: The Official Distributed Search & Analytics Engine​, n.d.)
https://www.elastic.co/elasticsearch/
3) (​NLTK (Natural Language Toolkit) Tutorial in Python,​ n.d.) https://www.guru99.com/nltk-tutorial.htm
4) (​Gensim (Python Documentation),​ n.d.) https://pypi.org/project/gensim/
5) (​Pyspark​, n.d.) https://databricks.com/glossary/pyspark
6) (​Tweepy Documentation — tweepy 3.9.0 documentation​, n.d.) http://docs.tweepy.org/en/latest/
7) (​Apache Kafka​, n.d.) https://kafka.apache.org/intro

