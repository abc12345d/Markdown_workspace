# News Search Engine
Provides users with accurate and relevant news articles as search results based on their queries.

---

In this project, I built a simple search engine from scratch using Python and its supporting library.
To begin, I pre-processed a collection of Financial Times news articles in preparation of implementing the search engine. Subsequently, I used the pre-processed texts to generate a Term-Document Matrix (aka positional inverted list).
Finally, I implemented various search functionalities that operate on the Term-Document Matrix:
- Boolean search (not, or, and)
- Phrase search
- Proximity search
- Ranked information retrieval system based on TF-IDF weighting.


# Example Outcome of Ranked IR system based on TF-IDF 
<img width="1168" alt="News_Search_Engine" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/3fae3063-578b-4041-b4ce-4deb71b5b652">

## Project Code

```PYTHON
from nltk.stem import PorterStemmer
import xml.etree.ElementTree as ET
import math

##### functions for text pre-processing #####
def tokeniser(context):
    '''Split on every non-letter characters. Returns a list of tokens'''

    non_letter_list = "!\"#$%&'()*+, -./:;<=>?@[\]^_`{|}~0123456789"
    # change all the occurances of non letter characters into space and upper case letter into lower case letter
    # split the whole paragraph into a lists of words by using space as separator
    non_letter_table = context.maketrans(non_letter_list, ' '*len(non_letter_list))

    return context.translate(non_letter_table).lower().split()

def stopword_remover(word_list):
    '''Remove all the occurances of stopword from the word list. Return a list of words.'''
    
    # read stop word list
    f = open("stopWord_Eng.txt","r")
    stopWord = [w.strip() for w in f.readlines()]
    f.close

    return [i for i in word_list if i not in stopWord]

def porter_stemmer(word_list):
    '''Stem the words in the word list by using Porter Stemmer. Return a list of stemmed words.'''

    porter = PorterStemmer()

    return [porter.stem(i) for i in word_list]
##### functions for text pre-processing #####


##### function for creating a positional inverted index #####
def generate_positional_inverted_index(tree_root):
    '''Implement positional inverted list which contains pre-processed term, list of documents where this term occured and list of positions where the term occured within the document for each document. Returns a positional inverted list of type 'dict'. '''

    token_dict = {}
    # Loop through all the documents in the collection
    for i in root.iter("DOC"):

        docNo = i.find("DOCNO").text
        # The document text should include the headline and text fields. 
        # For positions of terms, start counting from the headline and continue with the text. 
        docText = i.find("HEADLINE").text + i.find("TEXT").text


        # tokenize the document text, remove all the stop words and stem the remaining words
        tokens = porter_stemmer(stopword_remover(tokeniser(docText)))

        for counter in range(len(tokens)):
            ## t is a token
            t = tokens[counter]
            if t in token_dict:
                if docNo in token_dict[t]:
                    # position starts from 1 so (counter+1)
                    token_dict[t][docNo].append(counter+1)
                else:
                    token_dict[t][docNo] = [counter+1]
            else:
                token_dict[t] = {docNo : [counter+1]}

    return token_dict
##### function for creating a positional inverted index #####

##### functions for searching #####
def boolean_search_and(query1, query2): 
    '''Gets two lists of document ID and return a list of document ID that appeared in both lists'''
    return list(set(query1) & set(query2))

def boolean_search_or(query1, query2): 
    '''Gets two lists of document ID and return a list of document ID without repetition'''
    return list(set(query1) | set(query2))

def boolean_search_not(query1): 
    '''Return all the document ID of the collection except those appeared in the input list '''
    return list(set([int(i.text) for i in root.iter("DOCNO")]) - set(query1))

def is_close(pos_list1, pos_list2, distance, isPhrase):
    '''Gets two lists of positions and return True if there exists 2 positions (one from pos_list1 and the other from pos_list2) are close '''
    pos_list2.sort()

    # loop through all the position in pos_list1
    for pos1 in pos_list1:

        # Use binary search to find the nearest position in the pos_list2 to current pos1
        low = 0
        high = len(pos_list2)-1
        while high - low > 1:
            mid = (high + low)//2
            if pos_list2[mid] > pos1:
                high = mid
            else:
                low = mid

        # if isPhrase is True, check if pos2(from pos_list2) is right behind pos1(from the pos_list1)
        # else check if the distance between pos1 and the pos2 is equal to or less than distance
        if isPhrase:
            if (pos_list2[low] - pos1 == 1) | (pos_list2[high] - pos1 == 1):
                return True
        else:
            if min(abs(pos_list2[low] - pos1),abs(pos_list2[high] - pos1)) <= distance:
                return True
            
    return False

def proximity_search(inverted_index,stemmed_w1,stemmed_w2,distance):
    '''Returns a list of documents that have both stemmed_w1 and stemmed_w2, and distance between stemmed_w1 and stemmed_w2 is less than or equal to distance (after stop words removal)'''
    dict1 = inverted_index[stemmed_w1]
    dict2 = inverted_index[stemmed_w2]
    close_doc_list = []

    # loop through all the document IDs that contained both stemmed_w1 and stemmed_w2
    for i in boolean_search_and(dict1.keys(),dict2.keys()):

        # check if the position of stemmed_w1 is close to the position of stemmed_w2
        if is_close(dict1[i],dict2[i],distance,False):
            close_doc_list.append(int(i))
    
    return sorted(close_doc_list)

def phrase_search(inverted_index,stemmed_w1,stemmed_w2):
    '''Returns a list of documents that contained the phrase "stemmed_w1 stemmed_w2".'''
    dict1 = inverted_index[stemmed_w1]
    dict2 = inverted_index[stemmed_w2]
    close_doc_list = []

    # loop through all the document IDs that contained both stemmed_w1 and stemmed_w2
    for i in boolean_search_and(dict1.keys(),dict2.keys()):

        # check if the position of stemmed_w2 is right behind the position of stemmed_w1
        if is_close(dict1[i],dict2[i],1,True):
            close_doc_list.append(int(i))
    
    return sorted(close_doc_list)
##### functions for searching #####

##### Ranked IR based on TFIDF #####
def tfidf(n,tf,df):
    '''Returns the weight of term by using tf and IDF'''
    w = (1 + math.log10(tf))* math.log10(n/df)
    return w

def tf(inverted_index,t,d):
    '''Returns the number of times term t appeared in document d'''
    return len(inverted_index[t][d])

def df(inverted_index,t):
    '''Returns the number of documents term t appeared in'''
    return len(inverted_index[t])

def score(inverted_index,n,q):
    '''Ranked IR based on TFIDF. Returns a list of tuples (document ID,score) sorted by the score from largest score to smallest score'''
    score_dict = {}
    # loop through  f
    for t in q:
        # for each token, loop through all the documents contained the token
        for d in inverted_index[t].keys():
            if d in score_dict:
                score_dict[d].append(tfidf(n,tf(inverted_index,t,d),df(inverted_index,t)))
            else:
                score_dict[d] = [tfidf(n,tf(inverted_index,t,d),df(inverted_index,t))]
    
    # for each document, sum over the weights of terms
    for d in score_dict.keys():
        score_dict[d] = round(sum(score_dict[d]),4)
        
    # sort a list of tuples (document ID,score) by the score from largest score to smallest score
    return sorted(score_dict.items(),key= lambda x: x[1],reverse=True)
##### Ranked IR based on TFIDF #####

def output_index_file(inverted_index):
    '''write a formatted version of positional inverted index into a file named index.txt'''

    # Convert the dictionary to a string with specific format
    res = ""
    for t in inverted_index:
        res += t + ":" + str(len(inverted_index[t])) + "\n"
        for d in inverted_index[t]:
                res += ( "\t" + d + ": ")
                res += (",".join([str(i) for i in inverted_index[t][d]]))
                res += "\n"

    f = open("index.txt", "w")
    f.write(res)
    f.close()


# parse the collection stored in xml format into a tree
tree = ET.parse('trec.5000.xml')
root = tree.getroot()

# implement a positional inverted list for the collection
inverted_index = generate_positional_inverted_index(root)
# implement a positional inverted list for the collection
inverted_index = generate_positional_inverted_index(root)
noOfDoc = len(list(root.iter("DOC")))

output_index_file(inverted_index)

#### code used for getting final outcome for my portfolio website
query = "Dow Jones industrial average stocks"
tokens_query = porter_stemmer(stopword_remover(tokeniser(query)))
score_query = score(inverted_index,noOfDoc,tokens_query)

res = f'Query: {query}\n\n'
counter = 0
while (counter < 5) & (counter < len(score_query)):
    (docId,docScore) = score_query[counter]
    counter += 1

    res += f'docID: {docId} score: {docScore}\n'

    # find headline of docId from the collection
    for index in root.iter("DOC"):
        docNo = index.find("DOCNO").text
        if docNo == docId:
            docText = index.find("HEADLINE").text 
            res += f'headline: FT / {docText[17:]} \n'


f = open("results.ranked.txt", "w")
f.write(res)
f.close()
```

# Project report
<a href="https://abc12345d.github.io/file_server/ttds_report.pdf">If nothing shows up, click this link</a>
<object data="https://abc12345d.github.io/file_server/ttds_report.pdf" type="application/pdf" width="100%" height="1000vh">
</object>
