---
Tags: 
Created: 2023-01-18 05:57:22
---
(Links:: [[Web Technology]])
# Using Javascript with HTML
- **Document Object Model** is a data structure that represents all parts of an HTML document
- Javascript has access to the **window object** representing an open browser window
	- window.location: contains information about the window's current URL
	- window.navigator: information about the browser
	- window.innerHeight and window.innerWidth: height and width in pixels of the window's content area
- `<script>` tag's *async* attribute allows for processing the webpage and loading and processing the JavaScript
- `script>` tag's *defer* attribute allows for processing the JavaScript only after the webpage is completely loaded
# Document Object Model
- DOM tree is a visualization of the DOM data structure consisting of *nodes*; Tags are created for each element, the text between an element's tags, and the element's attributes
	- *root node*: node at the top of the DOM
	- *child node*: node directly under another node
	- *parent node*: node directly above another node
- **Searching the DOM**
	1. `document.getElementById()`
	2. `document.getElementsByTagName()` (returns array)
	3. `document.getElementsByClassName()` (returns array)
	4. `document.querySelectorAll()` (returns array)
	5. `documnet.querySelector()`
- **Modifying DOM node attributes**
	- Each attribute property name acts as both a getter and a setter
		- Getter: read the attribute's value
		  Ex: `nasaUrl = document.getElementById("nasa_link").href`
		- Setter: modify attribute
		  Ex: `document.getElementById("nasa_link").href = "https://www.spacex.come/"`
	- element's attribute can be removed using method `removeAttribute()`
	  Ex: `document.getElementById("nasa_link").removeAttribute("href")`
- **Modifying DOM node content**
	- `textContent`: gets or sets a DOM node's text content
	- `innerHTML`: gets or sets a DOM node's content, including all of the node's children
# More DOM modification
- `document.documentElement` is the root of the DOM tree
- node properties for accessing realted nodes
	- `parentNode`
	- `childNodes`: array-like collection
	- `children`: only element nodes and no text nodes
	- `nextElementSibling`
	- `previousElementSibling`
## Modifying the DOM structure
- `appendChild()`: appends DOM node to object's chlid nodes
  Ex: 
```javascript
ol = document.getElementsByTagName("ol")[0];
li = ol.getElementsByTagName("li")[0];
ol.appendChild(li);
```
- `insertBefore()`: inserts DOM node as a chlid node before an object's existing child node
- `removeChild()`: removes node from object's children
  Ex: `n.parentNode.removeChild(n)`
- `createElement()`: returns new element node
  Ex: `document.createElement("p")`
- `createTextNode()`: returns new text node containing the text specified by a string argument
  Ex: `document.createTextNode("Hello there!")`
- `cloneNode()`: returns identical copy of DOM node; method's boolean argument indicates whether the method should clone node's children
# Event-driven programming
- events caused by user-interaction
- Event-driven programming: code runs only in response to various events
	- code that runs in response to an event is called an *event handler* or *event listener*
- common events
	- *change* event caused by an element value being modified
	- *input* event caused when the value of an input or textarea element is changed
	- *load* event caused when browser completes loading a resource and dependent resources 
	- *DOMContentLoaded* event caused when HTML file has been loaded and parsed
	- *focus* event caused when element becomes the current reciever of keyboard input (ex: clicking on input field)
	- *blur* event caused when element loses focus / no longer recieves future keyboard input
	- *submit* event caused when user submits a form to web server 
- registering event handlers
	- embedding the handler as part of the HTML
	  Ex: `<button onclick="clickHandler()">Click Me</button>`
	- setting DOM node event handler property directly using JavaScript
	  Ex: `document.querySlector("#myButton").onclick = clickHander`
	- Using the JavaScipt *addEventListener()* method to regist an even handler for a DOM object
	  Ex: `document.querySelector("#myButton").addEventListener("click", clickHandler)`
	  The `addEventListener()` method allows for separation of content and functionality and allows multiple handlers to be registered with an element for the same event
- handlers have an optional *event object* parameter that provides details of the event
- preventing default behavior
	- process stopped by calling `stopPropagation()` method
	- `preventDefault()` method stops web browser from performing built-in handler (ex: clicking a form's submit button)
# Timers
- execute a function after a time delay using `setTimeout()`
	- two arguments: a function and a time delay in milliseconds
	- returns unique integer identifier as a timeout which can be canceled with `clearTimeout()`
- execute a function repeatedly with a time delay using `setInterval()`
	- two arguments: a function and time interval in milliseconds
	- functions called every $t$ milliseconds until interval is canceled using `clearInterval()`
# Modifying CSS with JavaScript
- Modifying inline style
	- CSS Object Model is a set of APIs that allow JavaScript to manipulate CSS properties of a webpage
	- `getPropertyValue()`: returns value of an element's CSS property or empty string if property is not set
	  Ex: `elem.style.getPropertyValue("color")`
	- `setProperty()`: `elem.style.setProperty("color", "blue")`
	- `removeProperty()`
	- CSS style properties can be modified using JavaScript property names ->
	  `elem.style.color = "blue"` == `elem.style.setProperty("color", "blue")
- Modifying a stylesheet with `document.styleSheets`
	- `insertRule()`: inserts a new rule into the stylesheet
	  Ex: `document.styleSheets[0].insertRule("p {color: blue;}")`
	- `deletRule()`: delets a rule at a given index number from the stylesheet
	  Ex: `document.styleSheets[0].deleteRule(0)` (deletes first rule)
	- `document.styleSheets[0].cssRules[0].style.setProperty("color", "blue")`
- Adding and removing classes
	- Every DOM node has a *classList* property that lists classes assigned to the node
	- `add()`: adds class to the element's list of classes
	  Ex: `elem.classList.add("mystyle")`
	- `remove()`: removes class form node's classList
	- `toggle()`: adds class if not present, else it's removed
# Form validation
- textual input and drop-down menu elements have *value* attribute that can be used to validate user-entered text by checking for properties
- checkboxes and radio buttons have *checked* attribute
- validating each field as data is entered
	1. for each field that should be validated
		1. Register an input event handler for the field
		2. Create a global variable to track whether the field is currently valid. In most cases, this global variable should be initialized to false since the form typically starts with the field as invalid
		3. Modify the global variable as appropriate within the field's event handler
	2. Register a submit event handler for the form that verifies the global variables for each field are true
	3. If one or more of the global variables are false, call the `preventDefault()` mehthod on the event to cancel the form submission and keep the form from submitting to the server
- HTML: customized elements automatically checked by the browser, ex: required, max / min, maxlength / minlength, pattern, title
	- styling with CSS: `:valid`, `:invalid`, `:required`, `:optional`
# JavaScript Object Notation (JSON)
- JSON has six basic data types
	- **String**: Unicode characters enclosed in double quotes
	- **Number**: integer or decimal
	- **Object**: Unordered list of zero or more name/value pairs ({})
	- **Array**: ordered list of zero or more JSON values, separated by commas and enclosed within brackets ([])
	- **Boolean**: true or false
	- **Null**: represents *nothing*
- build-in JSON object
	- `JSON.parse()`: creates JavaScript object from a string containing JSON
	  Ex: `let array = JSON.parse('[1,"two",null]')`
	- `JSON.stringify()`: creates string from JavaScript object
	  Ex: `JSON.stringify(new Date('2020-08-06'))` converts Date object to string `2020-08-06T00:00:00.000Z`
- Extending and customizing JSON output
	- `parse()` method has optional parameter for a reviver function (used to modify parsed values before being returned)
	- `stringigy()` method has two optional parameters; replacer and spacer
		- replacer enables customization of generated string
		  Ex: `JSON.stringify({a:1,b:2,c:3},["a","b"])` returns `'{"a":1,"b":2}'`
		- spacer controls the indentation spacing of output JSON string
# XMLHttpRequest (Ajax)
## Ajax introduction
- **Ajax** (**Asynchronous JavaScript and XML**) is a technique to communicate with a server and update a webpage one the response is recieved, without reloading the whole webpage
	- **XMLHttpRequest** is an object for communicating with web servers using Ajax; it allows web browsers to hide communication latency and continue to provide a responsive user interface while waiting for a server response
## Using XMLHttpRequest
- create XMLHttpRequest object
- Assign handlers to desired events 
- initialize a connection to a remote resource using `open()` method; `open()` takes two arguments: HTTP request type and URL for the resource
- modify default HTTP request headers if needed with `setRequestHeader()`
- send HTTP request via `send()`
````ad-example
title: Ajax request
```javaScript
function responseReceivedHandler() {
	console.log("handling response: " + this.responseText);
}
let xhr = new XMLHttpRequest();
xhr.addEventListener("load", responseReceivedHandler);
xhr.open("GET", "http://www.example.org/example.html");
xhr.send();
```
````
## XMLHttpRequest result handlers
- **load** handler is called when exchange between browser and server has completed
- **error** handler called when browser does not receive an appropriate response to a request
- **abort** handler called when browser is told to 
- **timeout** handler called if the browser takes too much time to fully receive a response to a request
## XMLHttpRequest progress handlers
- **loadstart** handler called when browser begins to send a request
- **loadend** handler called after browser receives the response
- **progress** handler called whlie a response is being received by the client
## Attributes for determining XMLHttpRequest success
- **status** attribute: numeric status code returned in the response
- **statusText** attribute: text describing status attribute
- [[Common HTTP response status codes]]
## Accessing Ajax response data
- **response** attribute: response body which is parsed by the browser according to the `responseType` attribute
- **responseText** attribute: plain text version of the response
- **responseXML** attribute: XML DOM version of the response
- `responseType` attribute set to -
	- "json": the browser parses the entire response as a JSON object and sets `response` attribute to the JSON object
	- "" or "text": browser leaves response unprocessed and the `reponse` attribute contains same value ar `responseText`
	- "document": browser assumes response is an XML document, and `response` attribute contains same value as `responseXML`
````ad-example
title: Forecast
```html
<!DOCTYPE html>
<html lang="en">
<head>
   <title>Weather Forecast</title>
</head>
<body>
   <p>
      <label for="zip">ZIP code:</label> 
      <input type="text" id="zip" maxlength="5">
      <button id="search">Search</button>      
   </p>
   <div id="forecast"></div>
</body>
</html>
```
```javascript
function getForecast() {
   let zipcode = document.getElementById("zip").value;
   let xhr = new XMLHttpRequest();
   xhr.addEventListener("load", responseReceivedHandler);   
   xhr.open("GET", "https://wp.zybooks.com/weather.php?zip=" + zipcode);
   xhr.responseType = "json"
   xhr.send();
}

function responseReceivedHandler() {
   if (this.status !== 200) {
      alert("Error making HTTP request");  
   }
   let html = "";
   if (this.response.success) {           
      html += "<h1>Forecast</h1>";
      html += "<ol>";
      for (let day of this.response.forecast) {
         html += `<li>${day.desc}: high is ${day.high}, low is ${day.low}</li>`;
      }
      html += "</ol>";
   } 
   else {
      html = `<h1>Error: ${this.response.error}</h1>`;
   }

   document.getElementById("forecast").innerHTML = html;
}

document.getElementById("search").addEventListener("click", getForecast);
```
````
## Monitoring uploads
- XMLHttpRequest object's **upload** attribute is an object for monitoring the status of the request; progress handler typically used
```javascript
function uploadProgressHandler(event) {
	if (event.lengthComputable) {
		console.log(event.loaded + " loaded from " + event.total)
	}
}
...
xhr.upload.addEventListener("progress", uploadProgressHandler);
```
# Using third-party web APIs
- *third-party web API* used by web applications to access data from other sources
- web API's require *API key*
	- key identifies what application is using the web API
	- limits number of requests in a time period
	- developers must agree to restrictions placed by the third party
```ad-definition
collapse: none
title: **RESTful web API**
web API that is called with a URL that specifies API parameters and returns JSON or XML containing API data
```
- [[Weather API]]
## Cross-origin requests
- calling third-party web API's requires cross-origin HTTP request (since web API is not hosted on the local website's web server)
	- **Cross-Origin Resource Sharing (CORS)**: specification for how browsers and servers should communicate when making cross-origin requests
	- **JSON with Padding (JSONP)**: technique to circumvent cross-origin restrictions
 ## Calling the Weather API from JavaScript (Example)
```javaScript
getWeather(90210);

function getWeather(zip) {
   let endpoint = "https://api.openweathermap.org/data/2.5/weather";
   let apiKey = "APIKEY";
   let queryString = "zip=" + zip + "&units=imperial&appid=" + apiKey;
   let url = endpoint + "?" + queryString;

   let xhr = new XMLHttpRequest();
   xhr.addEventListener("load", responseReceivedHandler);
   xhr.responseType = "json";
   xhr.open("GET", url);
   xhr.send();
}

function responseReceivedHandler() {
   let weatherInfo = document.getElementById("weather");
   if (this.status === 200) {
      weatherInfo.innerHTML = 
       "<p>Current temp: " + this.response.main.temp + " &deg;F</p>" +
       "<p>Desc: " + this.response.weather[0].description + "</p>" +
       "<p>Humidity: " + this.response.main.humidity + "%</p>";
   } else {
      weatherInfo.innerHTML = "Weather data unavailable.";
   }
}
```
# Promises
```ad-definition
title: Asynchronous function
collapse: none
A function that starts an operation and potentially returns before the operation completes
```
- **Promise object**: object representing the eventual completeion of the asynchronous operation
  Can be one of three states:
	- *Pending*: asynchronous operation is still running
	- *Fulfilled*: asynchronous operation has completed successfully -> *settled*
	- *Rejected*: asynchronous operation has ended in failure -> *settled*
		- state will not change when Promise object is *settled*
- single **executor function** parameter that executes in the background with two parameters:
	- `resolve`: called when state become fullfilled
	- `reject`: called when state becomes rejected
 ## `Promise.then()` method
 - called to request notifications about the state
 - two arguments passed in:
	 - function to be called if Promise is fulfilled
	 - function to be called if Promise is rejected
## Promise fullfillment values and rejection reasons
- 

---
References: