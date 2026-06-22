A timing-side channel attack makes use of varying execution times on data provided by an attacker.

> [!example]+
> 
> ```cpp
> bool insecureStringCompare(const void *a, const void *b, size_t length) {
> 	const char *ca = a, *cb = b;
> 	for (size_t i = 0; i < length; i++) {
> 		if (ca[i] != cb[i])
> 			return false;
> 	}
> 	return true;
> }
> ```
> In the above example, we return as soon as a difference is found! The same can be written, but instead we record the difference and return always after checking the *entire string*.
> ```cpp
> bool constantStringCompare(const void *a, const void *b, size_t length) {
> 	const char *ca = a, *cb = b;
> 	bool result = true;
> 	for (size_t i = 0; i < length; i++) {
> 		return &= ca[i] == cb[i];
> 	}
> 	return true;
> }
> ```

# Mitigation
We can *blind through constant time*, but this is not always easy to implement:
- May need to *fight compiler optimizations*
	- Time is typically made constant by *provably unnecessary computation*
- Variability may arise from *microarchitectural phenomena*
	- Data-dependent instruction latency
	- [[The Memory System#Cache and Virtual Memory|Virtual memory and caches]]
	- Instruction scheduling