#### Project Description

NewsClues is one of my project where I developed a NLP framework that prioritizes a least of High Risk people based on risk factor and score. All these people are involved in some type of financial crimes. I gathered a private database "World Check" from a company called Refinitive. * This database contains names of high risk people from around the world and a summary of each person describing why they are high risk. The Machine Learning Model that is part of this NLP framework focuses on identifying the keywords in the summary from database and comparing them against a set tier hierarchy which contains financial crimes that are classified as low, medium or high risk/importance. Based on the comparison the person is assigned a score which identifies the tier they belongs to. 

*Since the dataset is private, I can not upload it, that's why I have changed it to dummy data. You can see the dummy data in the file "sample_data.csv"

* For the development of model and scoring I used Python, NLTK, Scikit-learn and are also planning on using Gensim, Spacy, pre-trained word embeddings and regex.  
* To build a vocabulary of keywords associated with financial crime I scrapped related Wikipedia Pages and stored them in a CSV file and created a corpus. I then used the corpus to build Tf-Idf vectors for each crime to build the vocabulary.  
* After that, I built the word embeddings for each tier and compared it with the word embedding for each record in World-Check.
* For the final scoring: Each tier in the context model (high, medium, low) is scored separately by adding up the scores allocated to the Number of External Sources criteria as well as the Number of Linked Parties Involved Criteria.

### Natural Language Processing Framework

Following is the NLP framework that I developed:

![nlp](https://github.com/UtsavManiar/NewsClues/blob/main/diagrams/NLP%20Framework.png)

I scraped the Wikipedia pages related to each crime and generated the word embeddings.
* Scraped Wikipedia pages using BeautifulSoup library.
* Performed the pre-processing steps.
* Generated TF-IDF vector and merged the TF-IDF vector for each tier by only considering top 30 words for each crime.
* Generated TF-IDF weighted word embeddings and averaged the word embeddings for each tier. We used pre-trained word2vec model to generate the word embeddings.

NewsClues gives a prioritized list of records by comparing word embeddings generated from extracted words with the word embedding associated with each tier.
* Loading World-Check data which contains 84000+ records (only Canadian data is used.)
* Performed the pre-processing steps.
* Generate the word embedding for each record by tokenizing the text.
* Compare word embedding of each record with word embeddings of all 3 tiers using Cosine similarity.
* Return the risk rated World-Check data in the csv format.

### Scoring Mechanism

Following is the scoring mechanism that returns final prioritized list for each risk tier:

![score](https://github.com/UtsavManiar/NewsClues/blob/main/diagrams/Scoring%20Framework.png)

NewsClues counts the number of linked parties and number of external sources attached to the person, returning the prioritized list by assigning a score for each tier separately.
* Load the risk calculated World-Check csv file.
* Count number of linked parties and assign the score based on the ranges that were defined in the scoring algorithm section above.
* Count number of external sources the person is mentioned in and assign the score based on the ranges that were defined in the scoring algorithm section above.
* Add together both the generated scores and come up with a final score.
* Return the sorted list based on score. 
* Perform all the above steps for each tier separately. *
* Final output is a csv file that returns the ordered list of records.

*Each tier (High, Medium, Low) will contain its own sorted list of profiles based on scores.


### NewsClues Code Files

Following is the description of each Jupyter Notebook:

#### 1. WikiScraper.ipynb :
* This notebook scrapes wikipedia pages of each crime listed to generate the vocabulary

#### 2. Wiki_Preprocessing.ipynb :
* This notebook preprocesses and cleans the scrapped wikipedia pages to prepare them for generating Tf-Idf vector and Word embeddings

#### 3. Wiki_tfidf_embedding.ipynb :
* This notebook creates Tf-Idf vector from scrapped wikipedia pages and divides it into defined risk tiers. It then generates Tf-Idf weighted word embedding and averages word embedding for each risk tier

#### 4. Predict_WorldCheck_RiskTier.ipynb :
* This notebook predicts Risk Rating of each record in the World-check data and generates a csv file with Risk Rating assigned to each tier

#### 5. Scoring.ipynb
* This notebook caculates the score of each record for each predicted tier seperately by taking into account the count of linked parties involved and number of external sources the person is mentioned in and returns the ordered list



