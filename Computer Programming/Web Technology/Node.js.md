---
Tags: 
Created: 2023-01-26 11:54:37
---
(Links:: [[Web Technology]])
# Full-stack development (Node)
## Fron-end and back-end development
- **Client-side**: technologies that run in the web browser (HTML, CSS, Javascript)
- **Server-side**: techonologies that run on the web server (PHP, Node.js and databases)
- **full-stack developer**: experites in all aspects of a website or web application's development including client technologies, server technologies, data modeling and user interfaces
- [[Primary layers of the full stack]]
## Web hosting
- *web hosting company*: hosts others websites on the company's servers
  Factors to consider:
	- *Reliability*: uptime and backups
	- *Flexibility*: handling more users; *virtual private server*: autonomouse server that is hosted on a physical server with other VPS
	- *Security*: encrypting web traffic 
	- *Price*
## Server-side progarmming
- **dynamic web page** is generated on the web server when requested, typically personalized to the user who requested the page
- **Single Page Application (SPA)** is a web application that provides a similar user experience as a desktop application all in a single web page
	- initially loads all of the application's resources so subsequent user interaction results in loading small pieces of content dynamically
- **web API** is a collectino of functions that are invoked using HTTP
## Databases
- **relational database**: stores data in tables (relations)
	- **Structured Query Language**: language for creating, editing, selecting, and deleting data in a relational database
- **Non-relational databases**: different methods for storing data
	- Document database: For storing documents in JSON format with many levels of nesting. Ex: MongoDB.
	- Key-value database. For storing values that are associated with unique keys. Ex: Redis.
	- Object database: For storing objects created in object-oriented programming languages. Ex: Caché.
	- Column database: For storing and processing large amounts of data using pointers that link to columns distributed over a cluster. Ex: HBase.
	- Graph database: For storing graph structures with nodes and edges. Ex: Neo4j.
## Client-sie technologies
- **HTML preprocessor**: converts markup language into HTML
- **CSS preprocessor**: converts a CSS-like language into CSS
- **UI library**: library that creates UI widgets or simplify DOM manipulation
- **CSS front-end framework**: framework that uses CSS or CSS pre-processors to aid in developing responsive websites that work well on every screen size
- **compile-to-JavaScript langauge**: language that is compiled into JavaScript (Typescript)
- **JavaScript framework**: JavaScript environment that dictates the organization of the application's JavaScript to simplify many programming tasks
# Getting started with Node.js
## Introduction
- **Node.js** is a JavaScript runtime environment used to run server-side web applications
````ad-example
```javascript
const http = require("http");

http.createServer(function(request, response){
	reponse.writeHead(200, {"Content-Type": "text/html"});
	response.write("<h1>Hello, Node,js!</h1>");
	response.end();
}).listen(3000);
```
- **http module** allows to create simple web server
- `createServer()` method creates server to recieve and sent HTTP
- `listen()` starts server listening for HTTP request on a port
- imported with `require()`
````
- **package** is a directory containing one or more modules and a package.json file
	- package.json contains JSON that lists the package's name, version, license, dependencies and other pagkage metadata
## package.json and package-lock.json files
- Node.js projects use package.json to list informaiton about the project
- all project's dependencies can be installed with `npm install` if package.json exists
- package-lock.json ensures that the same dependency versions are always used when the project is installed on different machines
- [[Semantic versioning]]
# Express
- allows developers to create web servers with less code
````ad-example
```javascript
const express = require("express");
const app = express();

//serve static file from the public dir
app.use(express.static("public"))

//Start the web server
app.listen(3000, function() {
	console.log("Listening on port 3000...");
})
```
````
## Routes
- **Express route** is a specific URL path and an HTTP request method to which a callback function is assigned
	- callback function has *request* and *response* parameters which represent the HTTP request and response
````ad-definition
```javascript
app.method(path, callback)
```
- `app`: express instance
- `method`: HTTP request method (get, post, etc.)
- `path`: URL path
- `callback`: Callback function executed when the route matches
- callback function uses `res.send()` to send an HTTP response
````
````ad-example
```javascript
app.get("/hello", function(req, res){
	res.send("hello");
});
```
````
## Middleware
- **middleware function**: examines or modifies the `request` and `response` objects
  parameters:
	- `req`
	- `res`
	- `next`
	- express method `use()` enables a middleware function to execute
````ad-example
```javascript
const express = require("express");
const app = express();

const logRequest = function(req, res, next) {
	console.log('Request: ${req.method}');
	next();
};
app.use(logRequest);

app.get("/hello", function(req, res) {
   res.send("<h1>Hello, Express!</h1>");
});

app.listen(3000, function() {
   console.log("Listening on port 3000...");
});
```
````
[[Pupular middleware for express]]
# Express request data
## Query string parameters
- query string parameters (values after the "?") automatically stored as values in req.query object
````ad-example
`http://localhost:3000/hello?name=Frank&age=17`
```javascript
app.get("/hello", function(req, res) {
	const html =
		'<h1>Hello, ${req.query.name}!<h1>
		<p>You are ${req.query.age} years old.</p>';
	res.send(html);
});
```
````
## Posting form data
- form's data is URL-encoded and sent in body of HTTP request
- `express.urlencoded()` parses URL-encoded data in request data and add parsed values to `req.body` object
```javascript
app.use(express.urlencoded({extended: false}));
app.post("/hello", function(req, res) {
	const html = '<h1>Hello, ${req.body.name}!</h1>
		<p>You are${req.body.age} years old.</p>'
		res.send(html);
})
```
## Route parameters
- string near or at the end of the URL path that specifies a data value
- defined in route path with a colon before parameter name
- attached to `req.params` object
````ad-example
`http://localhost:3000/users/jblack`
```javascript
app.get("/users/:username", function(req, res) {
	const username = req.params.username;
	res.send(username);
})
```
````
## Route parameter middleware
- `param()` method defines parameter middleware that executes before the route is called
- fourth parameter contains the value assigned to the route parameter
# Pug
## Template engines (exam)
- creates dynamic web pages by replacing variables in a template file with specific values, creating HTML that is sent to the web browser
- *rendering*: combining data with a template
# Relational databases and SQL (Node)
## Introduction to relational databases
- **table (relation)**: collection of related data is organized into columns and rows similar to a spreadsheet
## Create Table
**primary key** is the column(s) that uniquely identify each row in the table
```SQL
CREATE TABLE students {
	stuId INTEGER AUTO_INCREMENT,
	name VARCHAR(45),
	gpa FLOAT,
	PRIMARY KEY (stuId);
};
```
- `stuId`: primary key

**[[Common data types in SQL]]**
## Insert statement
- Create new rows in a table with an INSERT statement
- Retrieve all rows that meet some criteria with a SELECT statement
- Update the data in a row with an UPDATE statement
- Delete rows from a table with a DELETE statement
```SQL
INSERT INTO students VALUES
	(123, 'Susan', 3.1)
	(456, 'Billy', 2.5)
```
## Select statement
- retrieves data from a table ('*' -> all columns)
- `WHERE`: like if statement specifying conditions that must be true for a row to be selected
	- `ORDER BY`: sort selected rows in ascending order
	- `COUNT()`: counts how many rows are returned by a SELECT statement
## Update and delete statements
```SQL
UPDATE students SET gpa = 3.9
WHERE stuId = 987;
```
```SQL
DELETE FROM students
WHERE stuId = 456;
```
# MySQL (Node)
# Creating RESTful web APIs (Node)
## RESTful web APIs
- **REST (representational state transfer)**: architectural style that describes how various components interact in a distributed hypermedia system
- implemetns CRUD (create, read/retrieve, update, delete) operations on resources using the mechanics of HTTP
- **resource**: data entity whose representation is accessible via a URL
## Express router
- create modular route callbacks for the web API endpoints with `express.Router` class
- endpoint: URL for a web API used to access ar resource
````ad-example
title: Express router sends JSON-encoded song to web browser
```javascript
const express = require("express");

const app = express()
const router = express.Router();

//GET request returns JSON-encoded song
router.get("/song", function(req, res) {
	const song = {
		title: "Uptown Funk",
		artist: "Bruno Mars",
		populairty: 10,
		releaseDate: new Date(2014, 10, 10),
		genre: ["funk", "boogie"]
	};
});

//All requests to API begin with /api
app.use("/api", router);
app.listen(3000);
```
````
# Third-party web APIs (Node)

---
References: