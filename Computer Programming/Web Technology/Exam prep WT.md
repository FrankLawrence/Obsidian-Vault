---
Tags: 
Created: 2023-02-02 12:26:55
---
(Links:: [[Web Technology]])
# Internet & Web basics
- Internet (see zyBook Chapter 1)
	- High-level history of the Internet starting in the sixties
	- IP, Internet Protocol, IPv4 versus IPv6, IP addresses, limitations
	- TCP, notion of port, connection, packets being reordered if delivered out of order, packets being resent if not acknowledged by receiver
	- UDP: also port, no resending, used by DNS
	- DNS: service used to convert hostnames to IP numbers
	- WWW: Original proposal by Tim Berners-Lee in late eighties, built on top of DNS and TCP/IP
- WWW (see zyBook Chapter 1)
	- Original proposal by Tim Berners-Lee, three key ingredients: URL, HTML, HTTP (also correct: the 3 keys ingredients listed by the zyBook: The Browser, HTML, HTTP)
	- URLs build on the success of DNS by reusing DNS hostnames
	- Many alternative schemes: not just http://, also ftp://, mailto:, https://, etc
# Content makrup: HTML
- Basic history of HTML
- The separation of content and style
- The notion of semantic markup
- The basic tree structure of an HTML document
- The semantics of the HTML elements listed in the table of the appendix to Assignment 1
- Element tags and attributes
- The treatment of undefined/misspelled attributes or tags
- The purpose of the `class` attribute
- The difference between block and inline elements
- Recognize good and wrong use of empty elements
- Recognize invalid HTML and CSS (e.g. code the validator would report syntax errors on)
- Recognize HTML that is valid but violates best practices (see zyBook 3.7)
# Style markup: CSS
- Motivation for a style-sheet language
- The three ways of defining style sheets: in a separate file, in the head of the HTML or as an attribute on the element in the body
- The way to refer from your HTML head to a style sheet defined in a separate file
- Basic structure of style rules
- The meaning of the most common selectors and style properties (i.e. the ones used in Assignment 1)
- notion of responsive web design: style sheets that adapt the document to the device of the user (as used in assignment 1)
- Cascading: prioritization of user, author and user agent style sheets
- Basics of the box model and the normal-flow layout
# Scripting: JavaScript, AJAX, the Fetch API and the DOM
- key advantages and disadvantages of JavaScript, typical examples of JavaScript usage
- the role of the DOM when manipulating Web documents using JavaScript
- the role of events, event handlers and host objects in JavaScript
- understand how JavaScript programs communicate with the world (no print statement etc)
- main differences between JavaScript and languages such as Python or C++
- concepts underlying development, testing and debugging of JavaScript code
- key advantages and disadvantages of AJAX technology, typical examples of AJAX usage
- understand examples of fetch() API code as used for Assignment 2
# HTTP Servers & REST
- The notion of a client/server network architectures
- Basic concepts underlying HTTP request and HTTP response messages
- Basic functionality of a HTTP server
- Notion of "Virtual Hosts" (hosting multiple web sites on a single server)
- Principles of REST network architectures
- Notion of "safe" and "idempotent" HTTP methods
- Notion of HTTP status codes
- Notion of "stateless" protocols, advantages and disadvantages
- Notion of web "cookies" and how they relate to HTTP state
- Notion of static and dynamic content
- Notion of the same origin principle and cross origin resource sharing
- Notion of server logging, its use in Web analytics, privacy issues
# Information retrieval basics and Web Search `ris:CheckboxCircle`
- tf.idf: Every word is assigned a weight for a document. Some words are more important than others
	- removes bias for larger documents
	- takes into account zipf's and heap's law through $\log$
	- accounts for how often it is used in all documents -> not an important term (ex: is, the)
- **Zipf's law**: The most frequent word will occur approximately twice as often as the second most frequent word, three times as often as the third most frequent word, etc.
- **Heap's law**: By scanning the text we will hit upon the *most* common words rather quickly, but we will (increasingly slower), continue to encounter (infrequent) *new* words.
- Information retrieval is the activity of obtaining information resources relevant to an information need from a collection of information resources
- Small world phenomenon: there are very few pages that link to many others and a lot of pages that link to very few other pages
- **Information need:** the topic about which the user desires to know more
- **Query:** what the user conveys to the computer in an attempt to communicate the information need
- **Relevance:** the user perceives the document as containing information of value with respect to their personal informaiton need
- **Precision:** What fraction of the returned results are relevant to the infromation need?
- **Recall:** What fraction of the relevant documents in the collection were returned by the system?
# Web science, social implications of Web Technology `ris:CheckboxCircle`
```ad-definition
title: Web Science
collapse: none
Field of study that addresses the issues as social computing, universal usability, and interdisciplinary strategies.
It studies the way people behave on the Web, the technology that enables it and the interation between the two
```
```ad-definition
title: Net neutrality
collapse: none
Network neutrality is best defined as a network design principle. The idea is that a maximally useful public information network aspires to treat all content, sites, and platforms equally
```
- Net neutrality
	- Proposition:
		- Internet was made a success by buisiness
		- Very unlikely that ISPs will restrict their users
	- Opposition:
		- Government regulation was always used for the web, even in the beginning
		- Telephone market had government regulation
		- not part of "reasonalbe network management"
		- ISP-contorl will limit innovation free press
- Linked Data: connect data across different mediums (email, databases)
	- structured data not documents
	- graph data
	- Rules of linked data:
		- use **HTTP URI** as **names for things**
		- When someone looks up a URI, provide useful informaiton, using the standards (RDF)
		- Include *links* to other URIs so that they can discover more things
	- Resource Description Framework (RDF)
		- Semantic Web standard for writing down data, information "HTML but for data": (Subject, Relation, Object) **"Triple"**
		  Ex: <Painting001, has_location, Amsterdam>
		- extends the linkig structure of the Web to use URIs to name the relationship between things as well as the two ends of the link -> structured and semi-structured data can be mixed and shared across different applications
- Re-decentralizing and the Web and Solid
	- Where the web was once a anarchist/democratic place it's now becoming more and more the domain of **a few large companies** and **governments** using **surveillance machines**
	- Social fix: awareness and behavioural change
	- Technical fix: Solid ("social linked data") is a proposed set of conventions and tools for building decentralized social applications based on Linked Data principles. Solid is modular and extensible and it relies as much as possible on existing W3C standards and protocols
# Evaluation `ris:CheckboxCircle`
- Research question: always a question; has practical and / or theoretical relevance
- Hypothesis: describes the assumed relation between at least two variables
  can be any attribute or property of humans, objects or systems
- An independent variable influences dependent variables
  Ex: Web layout influences number of visitors
- **Explorative (What is related)**:
	- what factors determine if people come back to visit a website a second time?
	- Is there a correlation between characteristics of my visitors and the types of erorors that they make?
- **Descriptive (What happens)**:
	- What percentage of people find my website through google?
	- What percentage by typing in the URL directly?
- **Explanatory (Why does it happen)**:
	- Does the addition of a login function mean that more people come back to my website a second time?
	- Did the revision of the link structure of my website make visitors find what they were looking for quicker?
- qualitative vs quantitative data
	- Qualitative: non numerical, e.g. words, text, pictures or objects
	- Quantitative: analysis of numerical data
- A/B testing: take two random groups and compare how they use the service and then choose the one which is better
# Accessibility `ris:CheckboxCircle`
- Perceivable
	- Provide text alternatives for any non-text content
	- Provide alternatives for time-based media
	- create content that can be presented in different ways without losing information of structure
	- Make it easier for users to see and hear content including separating foreground from background
- Operable
	- make all functionality available from a keyboard
	- provide users enough time to read and use content
	- do not design content in a way that is known to cause seizures
	- Provide ways to help users navigate, find content, and determine where they are

---
References: