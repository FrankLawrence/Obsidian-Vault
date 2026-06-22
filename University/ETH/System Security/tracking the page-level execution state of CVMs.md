Assume that a function consists of two blocks of code, located in different pages in [[DRAM]]:
```armasm
// page A
call authenticate
test eax,eax
je auth
...
```
```armasm
// page B
do authentication
...
ret
```

Since the CVM's Guest Physical Address has to be mapped to physical addresses via the hypervisors stage-2 table translation, the untrusted hypervisor can *remove* pages, which causes [[Page Fault]]s in the CVM. 
Code is **unencrypted**, so the hypervisor can see when the functions page is requested. After the code in page B has executed, and *before retrieving page A* to return to, the attacker injects their own value into `rax`.