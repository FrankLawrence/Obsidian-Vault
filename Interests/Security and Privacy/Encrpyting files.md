---
Tags: 
Created: 2023-05-06 12:35:11
---
(Links:: )
# GPG
```
gpg -c --no-symkey-cache --cipher-algo AES256 filename.txt
```
- To encrpyt an entire directory, first archive it with `tar -cf dirname.tar.gz dirname/`
- to unarchive use `tar -xf dirname.tar.gz`
- encrpyting a file returns a `filename.txt.gpg` file

---
References: