---
Tags: lecture 
Created: 2023-02-02 18:30:53
---
(Links:: [[Web Technology]])
# HTTP
- Request-response protocol in the client-server computing model
- Client: a user agent such as a Web browser
- Server: Application running on a computer hosting a website
- Client submits an HTTP request message to the server
- Server typically provides resources such as HTML files and other content
- Response contains completion status information
- HTTP request: sample header fields
	- Host: host name from requested URL (required)
	- User-Agent: e.g. type of browser sending the request
	- Accept: Acceptable MIME types and encoding for responses
	- Connection: value close tells server to close connection or not
	- Content-Type: MIME type of POST body, if applicable
	- Content-Length: bytes in body
	- Referer: URL of document containing link that resulted in this request
- MIME for content-types
	- standard identifier used to indicate the type of data that a file contains
- HTTP response:
	- *Connection, Content-Type, Content-Length*: see request
	- *Date*: date and time at which response was generated (required)
	- *Location*: alternate URI if status is redirection
	- *Last-Modified*: date and time of last modification of requested resource
	- *Expires*: date and time after which copy of resource will be out-of-date
	- *Etag*: unique identifier for this version of the requested resource
- Example HTTP 1.1 request
```HTTP
GET /HTTP/1.1
Host: www.few.vu.nl
```
- www.cs.vu.nl is hosted on the same machine by the same server software
- server amy need to send different reponses for different host names
 

---
References: