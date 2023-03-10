---
Tags: 
Created: 2023-01-09 10:26:39
---
(Links:: [[Web Technology]])
# HTML containers
- **container**: any part of a web document body with opening and closing tags -> helps organize and format content

| Container   | Description                                                                                              |
| ----------- | -------------------------------------------------------------------------------------------------------- |
| `<header>`  | Container for introductory content                                                                       |
| `<footer>`  | Container for content descriptive information about the webpage like author, copyright, or date modified |
| `<address>` | Container for person's or organization's contact information                                             |
| `<main>`    | Container for the document's primary content                                                             |
| `<section>` | Container for distinct parts of a document, such as a chapter                                            |
| `<article>` | Container for self-contained content that can be reused independently, such as a news article            |
| `<nav>`     | Container for content relating to website navigation                                                     |
| `<aside>`   | Container for content not directly related to the main topic of a document                               |
| `<div>`     | Generic tag for creating block containers                                                                |
| `<span>`    | Generic tag for creating inline containers                                                                                                         |
- A **block element** fills the width of the element's parent container and can contain other block elements, inline elements, and text
- An **inline element** fills the minimum space possible in the element's parent container and can only contain text or other inline element's
# Forms
- **`<form>`** allows to submit information from the user to the server
	- **action attribute**: where should the data be sent
	- **method attribute**: what type of HTTP request type is it
- **GET method**: submit information to a web server from a web browser by altering the URL of the HTTP request -> web server recongnizes that it's a GET request by adding a **query string**
- **POST method**: submit information to a web server from a web browser by sending information in the HTTP request body
- Example:
  ```html
  <form method="POST" action="http://example.org/">
    <input type="text" name="item">
    <input type="text" name="price">
    <input type="submit">
  </form>  
  ```
- **widget**: interactive component
  **`<input>`** allows the user to enter information
	- **type attribute**: indicates widget type
	- **name attribute**: name widget and sends value when widget's form is submitted
	- **id attribute**: gives widget unique identifier
	- **placeholder attribute**: text that first appears in a text widget
	- **value attribute**: default value for a widget
- **label**: displays descriptive text associated with specific widget
	- has a **for attribute** whose value matches the `id` attribute for the widget being labeled
	  <label for="name">Name:</label>
	  <input type="text" id="name">
- **text area**
  <textarea name="summary" rows="4" cols="50">To summarize...</textarea>
# Common form widgets
- <label for="box">checkbox: </label><input type="checkbox" id="box" checked> 
- <label for=radio>radio: </label><input type="radio" name="movie" value="ET">
- **Drop-down menu**
  <select name="sport">
    <option value="football">Football</option>
    <option value="baseball">Baseball</option>
    <option value="basketball">Basketball</option>
    <option value="hockey">Hockey</option>
    <option value="soccer">Soccer</option>
  </select>
- **List box**
  <select name="flagcolors" size="4" multiple>
    <option value="red">Red</option>
    <option value="orange">Orange</option>
    <option value="yellow">Yellow</option>
    <option value="green">Green</option>
    <option value="blue">Blue</option>
    <option value="purple">Purple</option>
    <option value="white">White</option>
    <option value="black">Black</option>
  </select>
- **Buttons**
  <button type="button" alt="home"><strong>Home</strong></button> 
- **Password**
  <input type="password" name="secret" size="20" maxlength="10">
- **Fieldset**
  <fieldset>
    <legend>Favorite person</legend>
    <input type="radio" name="person" value="Person 1" id="Person1">
    <label for="Person1">Ellen</label>
    <input type="radio" name="person" value="Person 2" id="Person2">
    <label for="Person2">Ellen</label>
    <input type="radio" name="person" value="Person 3" id="Person3">
    <label for="Person3">Ellen</label>
    </fieldset>
# Additional form widgets
- **Date picker**
  <input type="date" min="01/01/2024">
- **Color picker**
  <input type="color">
- **Number input**
  <input type="number" max="20">
- **Range input**
  <input type="range" value="90" min="20">
- **Combo box** (text box and drop-down menu)
  <input list="Artists"><datalist id="Artists">
    <option value="Pink Floyd">
    <option value="Coldplay">
    <option value="Phil Collins">
  </datalist>
- **Specialized text input**
	- url
	- tel 
	- email 
	- search 
- **Input attributes**
	- maxlength
	- minlength
	- max
	- min
	- pattern
	- required
	- step
- **Fallback**
	- mechanism that allows a webpage element to function correctly even if the browser does not support a particular element
- **Polyfill**
	- fallback using Javascript code that makes certain HTML features work on browsers that do not natively support those features
# Audio and video
- Attributes: `autoplay`,`controls`,`loop`,`muted`,`width`(video)
  <audio controls><source src="https://quicksounds.com/uploads/tracks/1881020439_307941579_1391908515.mp3"></audio>
- Iframe: embedded youtube videos
# `<script>` and `<style>`
- allows a webpage to include executable code
- `type` attribute indicates content type when it isn't JavaScript
- `src` attribute provides URL of an external file containing JavaScript code
- `<style>` tag allows the webpage to introduce presentational directives, usually CSS
	- placed prior to the `<body>` tag
# HTML developer guidelines
- Use closing tags (even when not necessary)
- Avoid self-closing tags
- Use quotes for attribute values
- Use double quotes
- Use boolean attributes concisely
- Use lowercase for all tags and attributes
- Start block elements on new lines
- Indent nested elements consistently
- Separate content from presentation and functionality -> use `<link rel="stylesheet" href="theme.css">` to distinguish CSS from HTML
- Use CSS for layout
- Validate HTML
---
References: