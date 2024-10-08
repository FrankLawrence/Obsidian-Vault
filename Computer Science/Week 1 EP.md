---
Tags: 
Created: 2024-09-09 15:34:21
---
(Links:: [[Equational Programming]])
1. $$\lambda x.0$$
2. $$\lambda x.\lambda y. 0$$
3. $$\biggl(\lambda x.\Bigl(\lambda y.\big(x(yz)\big)\Bigr)\biggr)\Bigl(\lambda x.\bigl((yx)x\bigr)\Bigr)$$
4. $$(\lambda x.\lambda y. xyzu(vw))\lambda x.y(xx)$$
5. 
```mermaid
flowchart TD
	a[@] --> h[@] --> b[λxy] --> c[@] --> d[@] --> e[x]
	d --> f[x]
	c --> g[y]
	h --> i[λzu] --> j[@] --> k[u]
	j --> l[@] --> m[u]
	l --> n[z]
	a --> o[λw] --> p[w]
```
1. Indicateforallpairsofλ-termsbelowwhetherornottheyareα-convertible:
	1. 

---
References: