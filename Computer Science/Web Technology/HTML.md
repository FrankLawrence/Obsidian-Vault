---
Tags: 
Created: 2022-12-27 00:24:16
---
(Links:: [[Web Technology]])
# HTML document structure
1. The `<!DOCTYPE html>` declaration instruct the web browser about what type of docuemnt follows.
2. The `<html>` opening and closing tags enclose everything but the  `<!DOCTYPE html>` declaration. 
3. The `<head>` opening and closing tags contain the document title, document metadata, and various other elements that are typically not displayed in the webpage.
4. The `<meta>` tag specifies metadata, which is data that describes the document's data. `<meta charset="UTF-8">` describes how characters are represented in the HTML document. Additional `<meta>` tags may be used to indicate when the document was saved, who the author is, etc.
5. The `<title>` opening and closing tags enclose the name of the document. The title is usually displayed in the browser's titlebar, is used by search engines, and is used for bookmarking.
6. The `<body>` opening and closing tags enclose all elements and content to be rendered in the browser.

- A **void element** is an element that only needs an opening tag
# Basic HTML tags
- **paragraph**: `<p>`
- browsers treat all sequences of whitespace as a single space between non-whitespace characters
- **line break**: `<br>`
- `<section>`: collection or related content
- **heading**: `<h1>,<h2>,<h3>,<h4>,<h5>,<h6>`
- **emphasized**: `<em>` (italicized)
- **strong importance**: `<strong>` (bold)
- `<cite>`: denotes title ex. book, song or movie
- `<mark>`: denotes important content that is highlighted
- `<b>`: text that needs attention
- `<i>`: text in an alternative voice
- `<u>`: text that appears differently from normal text -> underlined
# Comments
- portion of the document that is not displayed by the browser `<!--abcde-->`
# Lists
- **unordered list**: collection of items, `<ul>`
	- each **list item** is surrounded by `<li>` opening and closing tags
- **ordered list**: sequenced collection of items, `<ol>`
	- numbering scheme specified with **type attribute**
- **nested list**: list within a list item of another list
# Tables
- surrounded by `<table>` opening and closing tags
- **cell** is a location in the table at a specific row and column
	- `<tr>` tags create **table row**
	- `<th>` tags create a new table cell containing **table header** information about the data
	- `<td>` tags create a new table cell containing a **table datum**
	- `<caption>` defines a short description text for a table
- **colspan attribute** and **rowspan attribute** specify how many columns or rows a cell should span
- optional tags
	- `<thead>`: specifies table header
	- `<tbody>`: specifies table body
	- `<tfoot>`: specifies table footer
# Images
- `<img>`: displays an image in a webpage
	- **src attribute** specifies the URL
	- **alt attribute** provides a text description 
	- ex: `<img src="https://example.com/foto.jpg" alt="foto">`
- **width attribute** and **height attribute** are optional
# Links
- **anchor tag**: `<a>`, defines a hyperlink in a webpage
	- **href attribute** specifies the hyperlink's URL
	- ex: `<a href="https://wikipedia.org/">Wikipedia</a>`
- **absolute URL** is a complete URL, **relative URL** specifies the relative path to the web resource
	- `../` indicated the file is one directory above the current HTML document
- **fragment**: refer to a section by adding a hash tag `#`
- **id attribute**: creates a **fragment identifier** permitting URL to link directly to the id's
- **target attribute**: how the browser should display the link when clicked
	- `_self`: same tab or window
	- `_blank`: new tab or window
# Special characters
- **entity**: mechanism used for writing special characters or symbols
- **non-breaking character**
	- **non-breaking hyphen** `&#8209;`
	- **non-breaking space** `&nbsp;`

---
References: