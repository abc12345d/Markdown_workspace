# Gutenberg Book Search Website

A full-stack website with its primary functionality as a book search engine, 'Boogle.' This search engine enables users to search for books using diverse keywords such as author, title, and content. Boogle is powered by a NoSQL database containing 12,421,637 documents extracted from the paragraphs of 9676 English fiction books, crawled from [Project Gutenberg](https://www.gutenberg.org). 

---

This project was developed by a group of six individuals, including myself, who contributed to data-related operations such as cleaning data scraped from the Project Gutenberg website and performing data preprocessing in preparation for the search engine implementation.
Besides, I was also responsible for work related to databases, such as the design of database schema, setup and maintenance of the database and implementation of the database accessor module. [Link to project's Github repo](https://github.com/ttds2022-group32/Boogle_Search_Engine/tree/main)

## Tech stack
- React with Tailwind CSS
- FastAPI
- MongoDB
- Python packages (pymongo, nltk, etc.)

## Motivation

While several common PDF readers allow us to search certain contents within a book, they do not yet allow users to search specific contents across many ebooks or throughout an entire database. On the Gutenburg website, despite the fact that it seems to provide us with a full-text search, the service actually relies on *Yahoo!*, *Google* and *DuckDuckGo* to search for the text but uses neither its own search engine nor its database. Our search engine, Boogle, offers the service of searching the specific contents in all the books kept in our database and listing all the contents that match the search query. It will be incredibly beneficial to users who wish to search for specific material across multiple books, for example, when they wish to verify a saying and wish to identify its location with the least effort.

## Data Crawling and Scraping

To prevent flooding the target website with requests, we opt not to request more than one piece of material at a time. To ensure that only one request is made at a time, we first run the crawler, which generates a list of the URLs to crawl, eliminating the need for a tree-like search throughout the crawling process. Additionally, we develop a crawler that saves the HTML source code when it receives a response from a website, enabling us to scrape the data offline. 

The contents are scraped mostly using the Python package BeautifulSoup4. We inspect the HTML's format and extract the information by recognising the HTML attributes and tags that are related to it. The difficult task at hand is extracting paragraphs from HTML-formatted book content. Owing to the fact that each book page encodes paragraphs differently, we must pay special attention to the common aspect of the pattern utilised to represent these paragraphs. We begin by extracting all possible paragraphs using HTML tags and then filtering these paragraphs using our self-defined filtering method, which eliminates some noise that is included as paragraphs (like chapter number, etc.). The extracted data is immediately saved as structured JSON-style objects in our database. 

Due to the fact that the crawling and scraping processes are completely automated, scheduled scraping is feasible. Once the timer expires, the server will execute scripts that check to see whether any modifications have been made to the target website. Scripts will scrape the new contents whenever a change is detected.

## Frontend

We create a frontend website from scratch. JavaScript is used as the programming language. We use Tailwind CSS, which is a utility-first CSS framework for fast-developing custom user interfaces. When the frontend was completed, it was uploaded to a Google server using the gcloud service so that people could access it using a web browser. Due to the page constraint, we will not discuss the Frontend structure in detail because it is not directly connected to the course; instead, we will focus on the other components of this project. 

## Backend
### Server Design

As stated above, we decide to use a more modern approach to design the front end-user interface. Our backend server does not need to render the HTML and send it back as content for the display. Since the process of rendering and displaying can be done by the front end, the only job needed for the backend server is to return the appropriate data according to requests from the front end application. Since the task is straightforward, we avoid using whole web frameworks like Django. Instead, we use FastAPI, which provides an API-based server for front-end communication. 

We design the server to only accept the GET methods to the specific URL(domain) of the server as API calls since we need to protect our database from malicious attacks that use HTTP methods like POST, DELETE, and PUT. The API is carefully separated according to its function. For example, the system is designed to access the default search API which includes the function of query correction and query expansion. Once the user decides not to use these functions, the front-end application will call another API of the backend server. In this way, dividing functions into different APIs enables the code to be shorter and simpler, also easy to maintain and modify. Theoretically, the server is able to handle multiple requests from multiple frontend clients.

<img width="555" alt="Gutenberg_Book_Search_Website" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/953e10a9-d074-4aad-abb3-80d3cb09c694">

Figure 1. Diagram of Boogle Searching Engine

### Data Cleaning & Text Preprocessing

Due to the fact that the data is retrieved directly from the HTML source code, several record fields are structured incorrectly and include useless data. We begin by cleaning the data using Python's built-in string techniques to eliminate all occurrences of escape characters such as "\n", "\r", and so on. Additionally, we extract the published date from the string "2008-06-27T00:00:00+00:00" and save it as a Python DateTime object. We then execute text-preprocessing on our data to prepare it for search engine indexing, such as tokenization, stop word removal, and stemming.

Our tokenizer function accepts a string as input and returns a list of tokens. Given the fact that our data collection is restricted to English fiction literature, numbers and symbols have little significance in our search. As a result, we choose the Python String maketrans( ) and translate( ) methods to convert all non-alphabetic characters in the input string to spaces. The string is case-folded into lower case. Then, using space as a separator, we split the string into a list of tokens and return the list. Due to the tokenizer's lack of special handling abilities for symbols such as hyphen mark - or prime symbol ', *Hewlett-Packard* and *Kiehl's* will be split into two separate tokens. This ought to provide no difficulty for information retrieval, as queries including these words (*Hewlett-Packard* and *Kiehl's*) will be tokenized by the same tokenizer as well. If we set up special handling rules for particular symbols but the queries provided by users do not contain them, it may cause issues during the information retrieval stage unless we allocate additional memory to index both scenarios.

We delete stop words from the list of tokens using a premade English word list from the SMART (System for the Mechanical Analysis and Retrieval of Text) Information Retrieval System [(Salton and Lesk, 1965)](https://www.zotero.org/google-docs/?PyNW8Z), which was developed by Cornell University in the 1960s.

We utilise the PorterStemmer function from the NLTKPython library [(Loper and Bird, 2002)](https://www.zotero.org/google-docs/?JlrLVI) to reduce morphological variants of words to a single stem. For instance, this collection of tokens ['connection', 'connected', 'connects', 'connecting'] will be returned as ['connect', 'connect', 'connect', 'connect'].

### Database

#### MongoDB and PyMongo 
MongoDB, a document-oriented database, is chosen as the database for this search engine because it combines extensive query and analytical capabilities with ease of installation. Additionally, there is a Python distribution that includes tools for interacting with it, which makes database modification relatively simple.

#### Data structure 
Data is already in the database at the time of collection. Each paragraph in a book is considered to be a document and for each document, the following information is stored:

<img width="888" alt="Data schema" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/4756981c-92ab-4b9a-8968-a8d29d74c5bb">


However, some of the original format is not that convenient for subsequent analysis and query, so they are updated. Details can be found in the following form.

<img width="888" alt="data_modification" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/d8fe2700-b59d-4500-9b29-74bc1def9e5c">


### Inverted List and Field List

After each document is preprocessed and stored in MongoDB, our model creates two indexes from token lists: an inverted index and an index for performing field searches.

First is the list of the inverted index and we store it in a nested dictionary with the format below,

{' ': {' ': [  ]}}

The first key is the token and the value is a dictionary. The second key is the document ID, and the value of it is a list of the position. Here the position is the position index after preprocessing. We include the position to enable proximity search and phrase search.

The second one is a field list and we store it in a nested dictionary with the format below,

{' ': {' ': [" ", " ℎ ", " "]}

Same as the inverted list, the first key is the token and the value is a dictionary. The second key is the document ID and the value is a list of strings to show this token occurs in which fields. If “content” occurs, it means this token occurs in the content field. One thing that needs to be mentioned is that the author field is not preprocessed and the title field is preprocessed as the content field. These two lists are used for scoring the queries.

### Query processing

Once the backend receives a query string, such as “Shelock holmse”, the query is autocorrected by a spelling checker to “Sherlock holmes” or expanded to appropriate author name depends on query. and it sends the query to the query parser. The parser transforms the query into two token lists: the first one is a preprocessed tokens using the same procedure as described in the preprocessing part of this document. The second is a list of  tokens without preprocessing to perform an exact field search.

### Building indexes on disk with SPIMI

One problem with large datasets is that the produced inverted list and field list are also too large to be in memory. Therefore, we build the indexes using the SPIMImethod [(Manning, Raghavan and Schütze, 2009)](https://www.zotero.org/google-docs/?yK3KSR) with modification. The SPIMI method makes a single pass of the document and produces dynamic posting lists that could grow in size. It then sorts and stores the posting lists to disk when the memory is full. After the single pass, SPIMI merges the files generated on disk to a single inverted index by reading each of the files line by line, so the processing could fit in the memory of a single computer. We modified SPIMI so that when the memory is full, the algorithm writes to MongoDB and merges the posting lists directly if the posting list is already in MongoDB.

### Loading indexes back to memory

Indexing this large also cannot fit into the memory of a single instance. Therefore, we implement an index queue that loads only part of the indexes. When new queries come in and the memory reaches a threshold, the system pops the oldest node. The index queue can also be easily modified to use other cache replacement policies such as LRU.

### Document retrieval and Ranking

After the query is processed, first the list of tokens goes through the field index to check if the token is in our whole token dictionaries. If all tokens in the query are not found, just return search not find. If any token exists, start the scoring depending on their field.

#### Document retrieval

For content, supporting two search methods on preprocessed query with inverted index list.

TFIDF: first get the set of all documents containing any token in the query, then for each document, calculate the TFIDF score and sort them by scores. 

Phrase/Proximity search: the system intersects each token’s postings list and measures the distance of consecutive tokens’ positions. A tolerance of 2 is set, meaning tokens should appear no further than 2 words apart.

Supporting field search on a preprocessed query with a field index list is available for the title. Additionally, for writers, a field index list is provided to aid in searching on a query that has not been preprocessed.

Field Search: For each token in the query, check if the token occurs in that field, get the document and assign a fixed score to it. Finally, sum up the scores for each document and sort by scores. Lastly, check the document’s book\_id, which is used to indicate the document belongs to which book and delete repeated ones. This is to avoid returning the same books.

#### Ranking

The system collects all results returned by phrase search and is given a fixed score because it will be an exact match, top 20 results from TFIDF, 5 results from title field search and 5 results from author field search. Then the system would sum up the same documents’ scores and remove repeated documents and sort by sum up scores.

## Reference

[Loper, E. and Bird, S. (2002). NLTK: The Natural Language Toolkit. *arXiv:cs/0205028*. \[Online\]. Available at: http://arxiv.org/abs/cs/0205028 \[Accessed 14 March 2022\].](https://www.zotero.org/google-docs/?m1KoIy)

[Manning, C. D., Raghavan, P. and Schütze, H. (2009). *Introduction to Information Retrieval*. P.569.](https://www.zotero.org/google-docs/?m1KoIy)

[Salton, G. and Lesk, M. E. (1965). The SMART automatic document retrieval systems - an illustration. *Communications of the ACM*, 8 (6), pp.391–398. \[Online\]. Available at: doi:10.1145/364955.364990.](https://www.zotero.org/google-docs/?m1KoIy)

[*Project Gutenberg*. \[Online\]. Available at: https://www.gutenberg.org/ \[Accessed 11 March 2022\].](https://www.zotero.org/google-docs/?m1KoIy)![](images/Aspose.Words.c31673e0-76fd-4872-9152-25280fe38596.002.png)
