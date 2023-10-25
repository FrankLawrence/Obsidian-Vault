---
Created: 2023-10-05 20:39:17
---
# Properties
Hashes are **hard to reverse**: Given a hash $h$, it is hard to find message $m$ such that $hash(m)=h$. Its hard to fabricate a message that matches the hash: Given a message $m_1$, it is hard to find another message $m_2$ such that $hash(m_1)=hash(m_2)$. 
This prevents an attacker from creating a message with the same signature as a signed message.
# Types
## SHA
SHA-1 uses 160 bits. Collision resistance has however been recently broken, with considerable compute time. For this reason SHA-2 was developed and used in new protocols and cryptocurrencies. It can be set up with 224, 256, 384 or 512 bits. It will probably we enough for the next couple years, but in case it isn't we developed **SHA-3**. It eliminates some theoretical weaknesses of earlier hashes.