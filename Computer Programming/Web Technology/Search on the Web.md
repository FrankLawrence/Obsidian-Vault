---
Tags: lecture
Created: 2023-01-26 19:30:05
---
(Links:: [[Web Technology]])
# Search Engines
- Information retrieval (IR) is the activity of obtaining  information resources relevant to an information need from a collection of information resources.
- Classical Information retrieval
	- Navigational: The immediate intent is to reach a particular site
	- Informational: The intent is to acquire some information assumed to be present on one or more web pages
	- Transactional: The intent is to perform some web-mediated activity
- Search strategies:
	- Boolean (search) queries: information retrieval where we pose any query where terms are acombined with operators $AND$ $OR$ and $NOT$ -> viewed as a set of words
	- Ad Hoc Queries: system aims to provide documents from within the collection that are relevant to an arbitrary user information need, communicated to the system by means of a one-off, user-initiated query
	- Browsing
	- Social bookmarking
	- Recommender systems
	- Filtering systems
- how can a search return documents from all over the web?
	- indexing
	- multiple servers in parallel
	- pre-selection based on time/origin/query
- Engines use a kind of locally stored summary of each page -> *index*
- building an index:
	- Crawling: search engines follow links of pages they know of already
		- there are very few pages that link to many others (Hubs) and a lot of pages that link to very few other pages
		- deep web isn't being linked to / is not crawled at all
	- Preprocessing
		- remove html tags
		- tokenization ("I am walking" -> "I" "am" "walking")
		- remove stop words ("the", "it")
		- stemming (cars -> car)
	- building index
- Term document matrix: find a term in a corpus
	- 0/1 vector for each term
	- take vectors for search term and use bitwise $AND$
	- the more terms the bigger the matrix
	- alot of empty space; better representation?
	  -> we record only the 1 positions
- Inverted index
	- for each term *t* we store a list of all documents that contain *t*
	- identify each by a docID
	- multiple term entries in a single document are merged
	- split into dictionary and postings
	- Doc. frequecy information is added
- How well does a search engine work
	- select a representative set of queries
	- humans judge/rate search results -> subjective task
		- judge relevance with respect to information need
	- Precision and recall
		- *Precision*: How correct are the retrieved results? 
		  $$\texttt{precision}=\frac{|\{\texttt{relevant documents}\}\cap\{\texttt{retrieved documents}\}|}{|\{\texttt{retrieved documents}\}|}$$
		- *Recall*: How complete are the correct retrieved results?
		  $$\texttt{recall}=\frac{|\{\texttt{relevant documents}\}\cap\{\texttt{retrieved documents}\}|}{|\{\texttt{relevant documents}\}|}$$
		  ![[Precision and Recall.excalidraw]]
		- F-Precision: harmonic mean of precision and recall:
		  $$\texttt{F-measure}=2*\frac{\{\texttt{precision}\}*\{\texttt{recall}\}}{\{\texttt{precision}\}+\{\texttt{recall}\}}$$
- does it make sense to consider all retrieved documents?
	- when the number of results grows larger, it might not be relevant what the precision over the entire set is, but only *first N* results
- Ranking (how does google decide which link to show at the top)
	- tf.idf: Every word is assigned a weight for a document. Some words are more important than others
	  used heavily to find what topics link to a webpage
	- Zipf's law: The most fequent word will occur approximately twice as often as the second most frequent word, three times as often as the third most frequent word, etc.
	  Formally: *the frequency of a word is inversely proportional to its rank in the fequency table*
	- Heap's law: by scanning the text we will hit upon the *most* common words rather quickly, but we will, (increasingly slower), continue to encounter (infrequent) *new* words
	- other ranking tricks:
		- localisation (language, but also your mobile location)
		- personalisation
		- Log analysis
		- PageRank
			- Absolute score for a page
			- pages that are linked to by important pages are themselves important
## Summary
- web search is a form of **information retrieval** with Web as corpus
- **(Inverted) indexes** are built using **crawling, processing** and **indexing**
- A **boolean query** is then matched to the index, returning docs
- How well a search engine workd depends on **user judgement**
	- Precision, Recall and F-measure
- **Ranking is key** - especially in Web search
	- Good ranking metrics take **information laws** into account

---
References: https://docs.google.com/presentation/d/18y8hVdX2hxKqKrX0LdPXxXoXIXuayazUB9rszXkYjoc/edit#slide=id.p1