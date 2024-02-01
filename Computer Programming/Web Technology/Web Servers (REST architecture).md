---
Tags: lecture
Created: 2023-02-02 18:07:16
---
(Links:: [[Web Technology]])
# REpresentational State Transfer
- REST key principles
	- all pieces of information are resources that are uniquely addressable using a URI
	- Clients and servers need to agree on 3 key things to allow the client to interact with a resource on the server:
		- the URI of the resource, including parameters
		- the allowed actions on that resource
		- the allowed representations of the resource
	- Client does not need to know how the server generates the representation of the resource
	- Server does not need to know how the client renders the representation of the resource
	- Both client and server do not need to be aware of intermediate proxies or caches
	- There is no communication state
		- HTTP response should not depend on previous requests
		- HTTP GET mehtod should be safe: no side-effects
		- HTTP POST should be used when multiple identical requests have a different effect than a single request
		- All methods should be idempotent: requesting the same resource multiple times should yield the same result
- Same origin policy
	- Site A provides AJAX web services to support Web pages from site A
	- but does not want a script on a page from site b to also make call to site A
	- users might have used site A before, and site B might be trying to steal information from site A about the user
	- But requests to A come from the client, and A doesn't know if the client is running a script from A or B!
	- only the browser knows it is running a script from B calling A
	- So the browser will block calls to A on pages from 
	- Cross-Origin Resource Sharing (CORS)
		- HTML from site B often uses resources from A
		- harder to abuse than in the AJAX case
		- browser uses http header in AJAX request to A to tell A where the script came from
		- Server A uses http header in the AJAX reply to the browser to state if it is OK
- Security & error reporting
	- error messages could also reveal weknesses in server software
	- good practice: send error messages to server-side log, minimal reporting to client
- HTTP server sessions
	- how to recognize which requests belong to the same user?
		- look a client's IP address
		- in first response, send cleint a small but unique piece of data
		- ask client to send this back as part of the HTTP header of all following requests
		- piece of data is known as a cookie
- Cookies
	- privacy issues became serious issue in 1996 after publication in the Financial Times
	- All major browsers allow users to delete cookies and to be alerted when cookies are set
	- many site make privacy policies public on their site
	- Handy: personalisation -> user preferences & profile
	- Tricky: user tracking across websites, direct marketing

---
References: