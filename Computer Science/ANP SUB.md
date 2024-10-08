---
Tags: 
Created: 2024-09-09 23:49:30
---
(Links:: [[Advanced Network Programming]])
1. alloc,fresh: `len = 0`
   ![[75865.png|400]]
2. reserve(y): `len = 0`
   ![[3965.png|400]]
3. push(x): `len = x`
   ![[66707.png|400]]

Data transmission is always done from [HEAD -> len], see `netdev_transmit()` function. But packets need to build in *reverse* order. Hence, backward moving data pointer.
![[15836.png|500]]
1. In the [[ICMP]] layer you need to reserve the whole packet but only build ICMP data. So,
	1. `reserve(total size);`
	2. `push(ICMP size);`
**Very important to push in each layer to add `len`**
1. IP layer below assumes the reserve size, so only does further push
	1. `push(ip header size);` (see `ip_output()` function)
2. ETH layer's the same
	1. `push(ETH header size);` (see `netdev_transmit()`)

For further debugging enable output of all layer. Just because your header says 10 bytes does **not** mean 10 bytes were transmitted. Whenever in doubt, point the *pointers locations* in the sub so that you understand.

```c
printf("%p %p %d \n", sub->head, sub->data, sub->len);
```

---
References: