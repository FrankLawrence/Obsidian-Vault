---
Tags: 
Created: 2025-10-15 19:21:32
---
(Links:: [[System Security]])
# Side Channel Attacks Basics
- A side channel attack takes advantage of *leaked data*, be it through actively providing faulty values, or through passive listening, to deduce the inner workings of the system.
	- Timing of functions execution can be exploited to deduce input values
- simple analysis directly infers the meaning of leakages, where differential analysis requires many samples
- 
- For certain values of the plaintext message m and the decryption key d, certain functions are or aren't executed, leading to a difference in execution time 

> [!question] The square and multiply algorithm is commonly used to implement modular exponentiation. What makes RSA implementations that use this algorithm in its native form vulnerable to side-channel attacks?
> The algorithm computes the exponentiation by a series of squarings and multiplications. It processes the exponent bitwise. For each bit, a squaring is executed. If the current bit of the exponent is 1, the intermediate result is multiplied with the base $m$.
> This constitutes a branching based on the exponent (i.e., the key which must stay secret). Such secret-dependent branches leak information as they are reflected, for example, in the total computation time or system power consumption.
# RSA Timing Side Channel
- 

---
References: