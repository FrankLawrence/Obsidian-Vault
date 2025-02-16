**# Summary

  

# Subjects

[Exam topics](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#exam-subjects)
1. [Internet & Web basics](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#essentials)
2. [Content markup: HTML](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#html)
3. [Style markup: CSS](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#css)
4. [Scripting: Javascript and the DOM](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#javascript)
5. [HTTP servers](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#http)
6. [Web Search](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#ir)
7. [Web science](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#science)
8. [Evaluation](https://canvas.vu.nl/courses/78523/pages/exam-preparation)
9. [Accessibility](https://canvas.vu.nl/courses/78523/pages/exam-preparation?module_item_id=1383173#accessibility)

## Internet & Web basics

You will need to understand the following subjects conceptually:

#### Internet (see zyBook Chapter 1)
- High-level history of the Internet starting in the sixties
- IP, Internet Protocol, IPv4 versus IPv6, IP addresses, limitations
- TCP, notion of port, connection, packets being reordered if delivered out of order, packets being resent if not acknowledged by receiver
- UDP: also port, no resending, used by DNS
- DNS: service used to convert hostnames to IP numbers
- WWW: Original proposal by Tim Berners-Lee in late eighties, a hypertext system built on top of DNS and TCP/IP
- Hypertext based on earlier ideas such as Bush's Memex (1945), Nelson (who coined the word "Hypertext" in 1965), Engelbart's NLS (1968) and Apple's HyperCard (1987)
    
#### WWW (see zyBook Chapter 1)
- Original proposal by Tim Berners-Lee, three key ingredients: URL, HTML, HTTP (also correct: the 3 keys ingredients listed by the zyBook: The Browser, HTML, HTTP)
- URLs build on the success of DNS by reusing DNS hostnames
- Many alternative schemes: not just http://, also ftp://, mailto:, https://, etc

## Content markup: HTML

You will need to understand the following subjects conceptually:
- Basic history of HTML
- The separation of content and style
- The notion of semantic markup
- The basic tree structure of an HTML document
- The semantics of the HTML elements listed in the table of the appendix to Assignment 1
- Element tags and attributes
- The treatment of undefined/misspelled attributes or tags
- The purpose of the class attribute
- The difference between block and inline elements

In addition, we expect you to have acquired the following skills:
- You should be able to read and understand an HTML source document similar to the ones covered in the course
- Recognize good and wrong use of empty elements
- Recognize invalid HTML syntax (e.g. code the validator would report syntax errors on)
- Recognize HTML that uses valid syntax but violates best practices (see zyBook 3.7)
- You should be able to distinguish content and style markup in an HTML source text (see also the next exam subject).

## Style markup: CSS

You will need to understand the following subjects conceptually:
- Motivation for a style-sheet language

- [x] The three ways of defining style sheets: in a separate file, in the head of the HTML or as an attribute on the element in the body
- [x] The way to refer from your HTML head to a style sheet defined in a separate file
- Basic structure of style rules
- The meaning of the most common selectors and style properties (i.e. the ones used in Assignment 1)
- notion of responsive web design: style sheets that adapt the document to the device of the user (as used in assignment 1)
- Cascading: prioritization of user, author and user agent style sheets
- Basics of the box model and the normal-flow layout
- Responsive design, breakpoints and media queries (see ZyBook Chapter 6)
- You should be able to understand how particular style definitions lead to a particular rendering on the media screen and print.

In addition, we expect you to have acquired the following skills:
- You should be able to read and understand a CSS style sheet similar to the ones covered in the course, and detect errors in the style rules
- Recognize invalid CSS syntax (e.g. code the validator would report syntax errors on)

## Scripting: JavaScript, AJAX, the Fetch API and the DOM

Students are expected to conceptually understand:
- key advantages and disadvantages of JavaScript, typical examples of JavaScript usage
- the role of the DOM when manipulating Web documents using JavaScript
- the role of events, event handlers and host objects in JavaScript
- understand how JavaScript programs communicate with the world (no print statement etc)
- main differences between JavaScript and languages such as Python or C++
- concepts underlying development, testing and debugging of JavaScript code
- key advantages and disadvantages of AJAX technology, typical examples of AJAX usage
- understand examples of fetch() API code as used for Assignment 2

Students are expected to have the following practical skills:
- to read and understand elementary JavaScript code and JSON data examples, e.g. be able to understand the code used in the examples of the lectures and the JavaScript code used to make Assignment 2.

## HTTP Servers & REST (see slides of lecture 8 & 9);

Students are expected to conceptually understand:
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

Students are expected to have the following practical skills:
- to be able read and understand elementary HTTP request and response messages
- to be able read and understand HTTP server log fragments in the [Common Log Format](https://en.wikipedia.org/wiki/Common_Log_Format)
- to be able read and understand elementary node.js, express and JSON code fragments as discussed in the zyBook, slides & Assignment 3.

## Information retrieval basics and Web Search

- Being able to explain the difference between an information need and a search query, be able to define the notion of relevance in this context
- Be able to define the evaluations metrics precision and recall in relation to the notion of relevance
- understand the notion of precision at N, e.g. P@5
- Be able to describe the advantage of ranking based on tf.idf scores over ranking based on raw term counts
- Be able to explain how Zipf's law and Heaps' law related to word frequencies in natural text
- Be able to explain what a small world network is

See also the [slides of Lecture 7](https://docs.google.com/presentation/d/18y8hVdX2hxKqKrX0LdPXxXoXIXuayazUB9rszXkYjoc/edit?usp=sharing)

https://docs.google.com/presentation/d/18y8hVdX2hxKqKrX0LdPXxXoXIXuayazUB9rszXkYjoc/edit?usp=sharing on Web Search.

## Web science, social implications of Web Technology

Read and understand the article of [Schneiderman](http://www.cs.umd.edu/~ben/ShneidermanCACM6-2007.pdf)

https://canvas.vu.nl/courses/78523/pages/preparation-lecture-9-web-science-lecture-by-victor-de-boer 
understand the slides of the Web Science lecture: https://docs.google.com/presentation/d/1zjy6wXsHQI4CC6fL75Pgk0EooBt0-BvGjBavT4OpDck/edit?usp=sharing

- Understand Schneiderman's definition of Web Science.
- Be able to define net neutrality and summarize the key arguments of the parties in favor of net neutrality and those who oppose it.
- Understand the motivation for, and concepts underlying Linked Open Data
- Understand the motivation for, and concepts underlying re-decentralization of the Web and the Solid framework.

## Evaluation

- Understand the difference between a research question and a hypothesis
- Understand what dependent and independent variables are
- Understand what explorative, descriptive and explanatory research is
- Understand the difference between quantitative and qualitative research
- Understand what A/B testing is

Study the slides of  [Lecture 10 on Web Evaluation](https://canvas.vu.nl/courses/78523/modules/items/1383151).
## Accessibility (ZyBook Chapter 11) 

Understand main concepts from the [WCAG 2.0](https://www.w3.org/TR/WCAG20/)

https://www.w3.org/TR/WCAG20/: accessibility, accessibility guidelines, the principle of perceivability, impact of accessibility on website development.

In addition, we expect you to have some basic skills in assessing web accessibility based on your work  for the peer review of A1.

# Questions

## Internet & Web basics
1. You may arbitrarily pick any port to use for an application
2. There are finitely many top level domains
3. TCP builds on top of UDP by making packet transmission reliable
4. If the receiver of an IP packet is not in the local network, the packet is dropped
5. UDP is used in applications where speed is crucial
6. A metaphor for a client-server model would be a customer and a waiter of a restaurant
7. In the beginning, there existed HTML tags that only change the appearance of the document
8. DNS servers can convert domain names to IP addresses and vice versa
9. With IPv4 there are 256^4 different addresses
10. HTTP GET requests and POST responses usually contain data in the message body

## Content markup: HTML

1. `<p></p>` is an example of a void element
2. The document type is specified inside the html tag
3. The following document is valid html  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Confusing but Valid</title>
</head>
<body>
    <h1>Welcome</h1>
    <div>
        <p>This is valid.<!--<p>This is a hidden comment, not a paragraph.</p>--></p>
    </div>
    <div>
        <span><em>This is a block-level layout.</em></span>
    </div>
</body>
</html>
```

1. The class and id attributes can be used to style multiple elements
2. aside is considered semantic markup
3. It is considered good practice to put html headings inside section elements**
## Style markup: CSS
1. T `<link rel="stylesheet" type="text/css" href="mystyle.css">` is a correct way to refer to an external style sheet
2. T The heading will be coloured navy and centered
```css
/* mystyle.css */
h1 {color: navy;}
```
```html
<!DOCTYPE html>  
<html>  
	<head>  
		<style> h1 {color: orange;text-align:left}</style>  
		<link rel="stylesheet" type="text/css" href="mystyle.css">  
	</head>
	<body>  
		<h1 style="text-align:center;">This is a heading</h1>  
	</body>  
</html>
```
1. F The selector `section > div` applies to all divs inside a section element
2. F The margin provides space between the content and the border of an element
3. T The `@media` rule can be used to apply different styles depending on the size of the user’s device screen.
4. F The following CSS code contains errors `p {text-color:red; background:blue;}`
## Scripting: JavaScript, AJAX, the Fetch API and the DOM
1. `document.alert("Hello")` displays a dialog box with the message hello
2. The following HTML code contains 5 nodes in it's respective DOM tree
```html
<html>
	<title>Moby-Dick</title> 
	<body> 
		<p class="big">Call me Ishmael.  Some years ago...</p> 
		<p>There now is your insular city of the Manhattoes...</p> 
	</body>
</html>
```
1. `document.quertSelectorAll()` does not return all elements in the DOM
2. `2 + "3"` and `"2" + "3"` return the same result
3. `document.querySelector(“#myButton”).onclick` = clickHandler sets the click event handler for the element with id “myButton”
4. 5XX response codes are a general form for request redirection errors 
5. With the await keyword, execution continues, while the requested data is loaded**
## HTTP Servers & REST (see slides of lecture 8 & 9)
1. 
## Information retrieval basics and Web Search
1. Precision is a metric that describes the percentage of the *correctly* retrieved results to all retrieved results
2. "Precision at N" is often used to assign weights to words within a document
3. A disadvantage of term frequencies is that more common words are ranked equally as important as more unique words
4. In zipf's law, the frequency of a word grows logarithmically the higher the rank
5. An information need usually starts off with crafting a query
6. From 10 results only 6 documents were correct (from all 24 relevant documents). The recall is 0.75
7. "Precision at N" dictates that relevancy of retrieved documents increases with the number of results
## Web science, social implications of Web Technology
1. Net neutrality means that websites are rendered the same to everyone, regardless of their internet provider
2. One goal of Linked Open Data is to guarantee access to data for every individual
3. Resource Description Framework originally tried to link data on the internet similarly to HTML
4. The Web is considered a "small world" network, where the shortest path between two nodes (pages) is typically very large
5. The Solid framework aims to re-decentralize the Web by allowing users to host their data in PODs
## Evaluation
1. Interviews (primarily) fall under qualitative data
2. Confound variables affect *dependent* variables
3. When conducting experiments, it is best to choose a random group of people from the population
4. "Are users who spend more time on the product page more likely to complete a purchase?" is an example of an explanatory study
5. High-fidelity prototypes are often used to criticise the general concept of a product
## Accessibility (ZyBook Chapter 11)
1. Under WCAG 2.0 the **operable** principle dictates that users can view the content clearly
2. Many WCAG violations can be automatically identified using tools
3. Screen readers often makes use of style markup to help a user navigate a webpage
4. Purely decorative content does not require a textual alternative