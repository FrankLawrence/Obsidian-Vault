---
Tags: 
Created: 2023-01-20 02:33:40
---
(Links:: [[Web Technology]])
# Async and Await
## Async function
- declared with `async` keyword
# Fetch API
- method for sending HTTP requests and receiving HTTP responses
- sends GET request to the given URL argument and returns a Promise object; Promise object resolves to a **Response** object containing information about the HTTP response
  Properties and methods:
	- *response.status*: HTTP response's status code
	- *response.ok*: true if the HTTP response's status code is 2xx, false otherwise
	- *response.headers*: object containing the HTTP response headers
	- *response.text()*: returns a Promise that resolves with the textual body of the HTTP response
 
> [!example] Fetch request for a webpage
> ```javascript
> let url = "https://example.com/";
> let response = await fetch(url);
> console.log("status is " + response.status);
> 
> let html = await response.text();
> console.log(html);
> ```
> ```javascript
> let url = "https://learn.zybooks.com/";
> fetch(url).then(function(response){return response.text();}).then(function(html){console.log(html);})
> 	.catch(function(error){console.log("Err: ", error);});
> ```
## Fetching JSON
- `response.json()` method returns a Promise that resolves to an object created from parsing the response body as JSON

> [!example] Fetch request returning JSON
> ```javascript
> let url = "https://randomuser.me/api/?results=3";
> let response = await fetch(url);
> let users = await response.json();
> ```

## POST request
**Fetch Options**:
	- *method*: indicated HTTP method (GET,POST,PUT,DELETE)
	- *headers*: specifies various HTTP request headers (Ex: Content-Type and User-Agent)
	- *body*: specifies HTTP request body, which could be form data, a JSON-encoded string, or binary data

> [!example] 
> ```javascript
> let form = document.querySelector("form");
> form.addEventListener("submit", async function(e) {
>    e.preventDefault();
>    let response = await fetch("login", {
>       method: "POST",
>       headers: {
> 	      "Content-Type": "application/json"
>       },
>       body: new FormData(form)
>    });
>    
>    let result = await response.text();
>    alert(result);   
> });
> ```
> **FormData** object stores key/value pairs from a form submission
# Web storage
# Regular expressions
# Classes
# Classes (ES6)
# Classes (ES13)
# Inner / Outer functions and scope
# Closures
# Modules
# Strict mode
# Canvas drawing


---
References: