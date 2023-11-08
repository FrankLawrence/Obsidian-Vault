---
Tags: 
Created: 2023-11-05 01:12:59
---
Imagine two parties (Bob and Alice) would like to securely and privately communicate together. 
First Bob generates a *public* and *private key*. The public key $e$ is available to anyone. The private key $d$ is kept secret. He can use an encryption function $E_{k}(m)$ for a key $k$ such that $E_{d}(E_{e}(m))=m$. 

RSA is based on modular arithmetic. When generating the key, we generate a large number $m$ and a public key $e$ and private key $d$ such that $(n^{e})^{d}=n (\mod m)\quad \forall n$.
Computing $d$ from $e$ and $m$ requires writing $m$ as a product of prime number factors. This is known to be *very hard*.

After Alice has (correctly) received Bob's public key, she encrypts the message by computing $E_{e}(m)$. Alice sends the encrypted message to Bob. Since Bob knows the private key $d$ he computes **$E_{d}(E_{e}(m))=m$**, and can read the message contents.
RSA takes advantage of the [[Diffie-Hellman Key Exchange.canvas|Diffie-Hellman Key Exchange]]


---
References: