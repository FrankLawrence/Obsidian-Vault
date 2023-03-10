---
Tags: 
Created: 2022-12-26 14:14:08
---
(Links:: [[Web Technology]])
# IP addresses, domain names, URLs
- An **IP (Internet Protocol) address** is a computers unique address on the internet. **IPv4** has 32-bit addresses. **IPv6** uses 128-bit addresses.

- A **domain name** is a name for an IP adress. Capitalization doesn't matter. A **DNS (Domain Name System) server** converts the domain name to an IP adress. Computers and ISPs keep reference to the main DNS servers.

- A domain name belongs to one of numerous  **top-level domains (LTD)** such as .com, .net, .org, ... Each country is assigned a two-letter **country code top-level domain (ccTLD)**

- A **URL (Uniform Resource Locator)** is the location of a web resource on the web. A **web resource** is any retrievable item like a HTML file or image
  A URL composes of:
	- **Scheme**: characters at beginning followed by `://`
	- **Hostname**: complete domain name
	- **Path**: characters to the right of the hostname
	- **Query string**: Optional characters to the right of the question mark that provide for the web server
	- **Fragment**: Optional characters that start with a has character and refer to a certain location within a webpage

- Webpage errors
	- Domain name is not found by a DNS server -> "Sorry, the website `www.xyz.blahblahblah` cannot be found."
	- Web server is not repsonding
	- Specific requested page isn't found -> `404`
 
# HTTP
- **HyperText Transfer Protocol** is a networking protocol that runs over **Transmission Control Protocol / Internet Protocol (TCP/IP)** and governs communication between web browsers and web servers. TCP/IP is a protocol that governs how data packets are transferred over the internet from one machine to another
- HTTP functions as a request-response protocol between web browsers and web servers
	- **HTTP request**: message sent from the web browser to the web server (asking to send web resource)
	- **HTTP response**: message sent from web server back (web resource)

- HTTP request and response are composed of:
	- **Start line**: specifies HTTP version
		- request: request type and path
		- response: status code and phrase
	- **Header fields**: keyword followed by a colon and value; supply additional information about response and request
	- empty line
	- **Message body**: contains data being transferred between a web browser and server

- **request method** indicated the desired action to perform on a resource (ex. `GET`)
- **[[Common HTTP response status codes|status code]]**: three digit number that indicates status of requested resource (successful: 200, *browser redirect*: 301) 
 - **browser caching**: web content can be stored by the web browser on the computers' file system for quick retrieval later -> network traffic reduced
 - **network sniffer** is software that monitors network traffic and allows to inspect HTTP requests and responses
 - HTTPS encrypts HTTP traffic
	 - **Transport Layer Security (TLS)**: uses asymmetric public keys
	 - **digital certificate** by trusted certificate authority contains public key used by TLS
- Steps in an HTTPs transaction
	- Step 1: Browser requests an HTTPS connection to a webpage
	- Step 2: Web server sends digital certificate to the browser
	  The browser warns the user if the digital certificate is not from a trusted certificate authority
	- Step 3: The browser and web server initiate "TLS handshake"
	  The browser and web server use the TLS handshake to generate session keys used to encrypt and decrypt information. A "handshake" is a networking term that means two participants communicate back and forth in order to negotiate a solution
	- Step 4: The browser and web server transmit encrypted information
	  The encrypted information can only be decrypted by the browser and web server
# Web trends
- The *Internet of Things* (abbreviated as IoT) is the global collection of communicating devices that sense and control technology on behalf of humans. IoT devices range from a simple temperature sensor to a satellite-based laser scanner used to discover archaeological sites hidden by vegetation.
- *Web accessibility* is the ability of users with disabilities to access and use a webpage with reasonable effort
- *Cognitive computing* is the use of artificial intelligence techniques and access to vast amounts of data to simulate human problem solving in complex situations with ambiguity, changing data, and even conflicting information. 
- Separation of concerns is the design principle of breaking up web content using distinct languages and documents that overlap as little as possible. (CSS, HTML and Javascript)
- [[Important mobile development topics]]
# Introduction to HTML
- As an analogy, humans have similar components: structure (bones, organs, central nervous system), identifying attributes (eye color, hair style, height), and behaviors (brushing teeth, slam dunking a basketball).
- An HTML document is constructed with elements. An *element* is a single HTML structure that is represented with HTML tags.
  
  A *tag* has a descriptive name surrounded by < and > characters that the web browser uses to display content. Ex: `<p>` specifies a paragraph. Most HTML elements have an opening and closing tag. Ex: `<strong>always</strong>` has an opening tag `<strong>` and closing tag `</strong>`. Some tags, like `<img>`, do not require a closing tag.
# Introduction to CSS
- **Cascading Style Sheets** (CSS) is a textual language for describing how a webpage is styled for visual presentation. CSS controls the look and layout of webpage content.
- A **CSS rule** specifies styling properties for specific HTML elements. CSS rules may be placed within `<style>` tags in the HTML file's head part. Each rule indicates the element to be styled like h1 (header1) or p (paragraph), followed by a list in braces { } of property:value items like `color:blue`.
# Introduction to JavaScript
- **Javascript** is a programming language that runs in a browser, enabling webpages supporting actions like responding to a button click. JavaScript can be included in the HTML file's head or body parts.

---
References: