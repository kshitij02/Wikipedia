# Wise - WikiPedia-Search-Engine

This repository consists of a search engine over the 75GB Wikipedia dump. The code consists of index.py and search.py. Both simple and multi field queries have been implemented. The search returns a ranked list of articles in real time.

## Indexing:
* Parsing: SAX Parser is used to parse the XML corpus.
* Casefolding: Converting Upper Case to Lower Case.
* Tokenisation: It is done using regex.
* Stop Word Removal: Stop words are removed by referring to the stop word list returned by nltk.
* Stemming: A python library PyStemmer is used for this purpose.

After performing the above operations we create Intermediate Index files like 0.txt,1.txt,2.txt and so on. Once we are done with creating this intermediate file we perform a K-way merge to merge all these intermediate files into a single Index file.
Each entry in the big Index file is a word along with its posting list.
For quick retrieval of the Title's corresponding to a query I have created a Document ID - Title Mapping which can be loaded into the memory while performing the Search operation.

## Searching:
* The query given is parsed, processed and given to the respective query handler(simple or field).
* One by one word is searched using a temporary Offset file in order to retrieve it's corresponding posting list.
* Once we retrieve the posting list we note down it's frequency in different document and weigh them accordingly based on         their presence in different section within a document and rank them using TF-IDF Scores.
* After Ranking we return the top 10 documents based on their TF-IDF Score using the Document-ID Title Mapping.

## Files Produced:
* index.txt : It consists of words with their posting list. Eg. d1b2t4c5 d5b3t6l1
* DocId_Title_mapping.txt : It is basically a mapping of Document ID and the titles of a WikipediaPage
* temp_offset : A directory containing multiple files where each file contains the offset locations i.e. where a word is          located in the index.txt file. 

## How to run:
* To create the Index File : python index.py /path-to-xml-dump  /path-where-you-want-to-create-index-file
* For searching : python search.py /path-where-you-index-file-is-created

## Requirements:
* Python 3.x
* PyStemmer
* XML SAX Parser
* NLTK
